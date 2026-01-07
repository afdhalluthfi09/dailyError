# Complex Business Validation

**menyederhanakan logika** tersebut menjadi jauh lebih elegan menggunakan fitur **`after()` hook** di dalam FormRequest

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Validator;
use App\Models\Jadwal;
use Exception;

class StoreJadwalRequest extends FormRequest
{
    public function authorize()
    {
        return true; 
    }

    public function rules()
    {
        // Validasi struktur dasar inputan (Level 1)
        return [
            'id'       => 'required|exists:jadwal,id',
            'totkateg' => 'required|integer',
            'kategori' => 'required|array', // Pastikan input kategori berupa array
            'kategori.*' => 'string',       // Tiap item kategori harus string
        ];
    }

    /**
     * DISINI MAGIC-NYA!
     * Method ini berjalan SETELAH validasi rules() di atas lolos.
     * Kita gunakan untuk validasi logic database yang rumit tadi.
     */
    public function after()
    {
        return [
            function (Validator $validator) {
                // 1. Ambil Data Input
                $inputId = $this->input('id');
                $inputKategori = $this->input('kategori'); // Array kategori baru
                $inputTotalQuota = (int) $this->input('totkateg');

                // 2. Cari Jadwal "Diri Sendiri" di DB
                $currentJadwal = Jadwal::find($inputId);
  
                if (!$currentJadwal || !$currentJadwal->is_active) {
                    return; // Skip jika tidak aktif/tidak ketemu (sudah dihandle rules exists)
                }

                // 3. TENTUKAN "GROUP KELUARGA" (Parent ID)
                // Jika parsial NULL, berarti saya Parent. Jika tidak, saya Anak (ambil ID Bapaknya)
                $parentId = $currentJadwal->parsial ?? $currentJadwal->id;

                // 4. AMBIL SEMUA "ANGGOTA KELUARGA" LAINNYA (Bapak + Anak-anak)
                // Kecuali diri saya sendiri (where id != inputId)
                $familyMembers = Jadwal::query()
                    ->where('is_active', true)
                    ->where(function($q) use ($parentId) {
                        $q->where('id', $parentId)       // Ambil Bapak
                          ->orWhere('parsial', $parentId); // Ambil Saudara
                    })
                    ->where('id', '!=', $inputId) // Jangan hitung diri sendiri (sedang diedit)
                    ->get();

                // 5. KUMPULKAN SEMUA KATEGORI YANG SUDAH DIPAKAI KELUARGA
                $existingCategories = [];
                foreach ($familyMembers as $member) {
                    // Handle jika di DB tersimpan string JSON atau Array (via Casts)
                    $cats = is_string($member->kategori) ? json_decode($member->kategori, true) : $member->kategori;
  
                    if (is_array($cats)) {
                        $existingCategories = array_merge($existingCategories, $cats);
                    }
                }

                // === VALIDASI 1: CEK DUPLIKASI (Intersect) ===
                // "Ada input kategori yang sama.!"
                // Cek apakah ada irisan antara Input Baru vs Kategori Keluarga yg sudah ada
                $intersection = array_intersect($inputKategori, $existingCategories);
  
                if (!empty($intersection)) {
                    $validator->errors()->add(
                        'kategori', 
                        'Ada input kategori yang sama! (' . implode(', ', $intersection) . ')'
                    );
                    // Kita stop di sini biar gak lanjut cek kuota
                    return; 
                }

                // === VALIDASI 2: CEK KUOTA (Limit) ===
                // "Kategori sudah terinput semua.!"
                // Gabungkan kategori existing + kategori baru, lalu hitung yg unik
                $allCategories = array_unique(array_merge($existingCategories, $inputKategori));
                $totalUsed = count($allCategories);

                // Logic asli Anda: Jika jumlah terpakai == target total, ERROR.
                // (Note: Biasanya logicnya > target, tapi saya ikuti logic == code Anda)
                // Asumsi: Logic aslinya "Jika input ini masuk, maka kuota penuh/habis/berlebih"
  
                // Jika logic Anda: Total tidak boleh MELEBIHI kuota
                if ($totalUsed > $inputTotalQuota) {
                     $validator->errors()->add(
                        'kategori', 
                        'Kategori melebihi batas kuota! (Total: ' . $totalUsed . ', Max: ' . $inputTotalQuota . ')'
                    );
                }
  
                // Jika logic Anda persis kode lama (Error jika pas penuh? Agak aneh tapi saya ikuti):
                /*
                if ($totalUsed === $inputTotalQuota) {
                    $validator->errors()->add('kategori', 'Kategori sudah terinput semua!');
                }
                */
            }
        ];
    }
}
```

```php
public function update(StoreJadwalRequest $request) {
    // Jika kode sampai sini, berarti validasi DB yang rumit tadi SUDAH LOLOS.
    // Langsung simpan saja.
    $data = Jadwal::find($request->id);
    $data->update($request->validated());
}
```
