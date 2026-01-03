# hubungan DTO dengan class handleFormRequest apakah sama ?

Mari kita bedah kenapa alur ini adalah "Best Practice" dan apa perbedaan mendasar tanggung jawab (responsibility) antara keduanya, agar Anda semakin yakin.

### 1. Perbedaan Peran (Analogi Satpam & Koper)

Bayangkan kantor Anda adalah  **Aplikasi** .

* **Controller** adalah Resepsionis.
* **Service** adalah CEO (yang kerja mikir bisnis).

#### **A. FormRequest = Satpam (Security Guard)**

* **Posisi:** Di gerbang paling depan.
* **Tugas:** Memeriksa kelengkapan syarat **SEBELUM** masuk kantor.
* **Contoh:** "Mana KTP?", "Apakah bawa senjata tajam?", "Apakah emailnya formatnya benar?", "Apakah password minimal 8 karakter?".
* **Power:** Dia punya akses ke Database untuk cek `unique` (misal: email sudah terdaftar belum). Jika syarat tidak lengkap, user **ditendang** langsung di gerbang (422 Unprocessable Entity). Data belum sempat masuk ke Controller/Service.

#### **B. DTO = Koper Dokumen (Secure Briefcase)**

* **Posisi:** Setelah lolos Satpam (FormRequest).
* **Tugas:** Membungkus data yang sudah lolos pemeriksaan agar rapi, aman, dan mudah dibawa oleh Resepsionis (Controller) ke ruangan CEO (Service).
* **Contoh:** Memastikan data "15000" (string) diubah jadi `15000` (integer), memastikan nama variabelnya `$data->price` bukan `$data['harga_barang']`.
* **Power:** Dia tidak menolak data, dia hanya **menstandarisasi** data.

---

### 2. Mengapa Alurnya `FormRequest` -> `DTO`?

Anda benar, `FormRequest` jauh lebih *powerful* untuk validasi input (terutama yang berhubungan dengan Database dan HTTP). DTO sebaiknya "bodoh" (hanya menyimpan data), jangan dibebani logic validasi yang rumit.

**Visualisasi Alur:**

1. **User** kirim Form Input.
2. **`FormRequest`** menangkap & Validasi (`rules()`).
   * *Gagal?* -> Auto Redirect Back / Error JSON.
   * *Sukses?* -> Lanjut ke Controller.
3. **Controller** menerima `$request` yang valid.
4. **Controller** mengubah `$request` menjadi  **`DTO`** .
5. **Service** menerima **`DTO`** untuk diproses.

---

### 3. Implementasi Kodingan (Pembuktian)

Mari kita lihat bagaimana keduanya bekerja sama tapi tidak saling tumpang tindih.

#### Tahap 1: FormRequest (Validasi Liar)

Fokus: Cek aturan, format, dan database.

```php
// app/Http/Requests/StoreKelasRequest.php
class StoreKelasRequest extends FormRequest
{
    public function authorize() { return true; }

    public function rules()
    {
        return [
            // Validasi ini POWERFULL, DTO tidak bisa melakukan 'unique' semudah ini
            'name' => 'required|string|max:255|unique:kelas,name',
            'teacher_id' => 'required|exists:teachers,id',
            'harga' => 'required|numeric|min:0',
        ];
    }
}
```

#### Tahap 2: DTO (Transformasi & Type Safety)

Fokus: Mengubah data yang sudah valid menjadi Object PHP yang pasti.

```php
// app/DTOs/KelasDTO.php
class KelasDTO
{
    public function __construct(
        public string $name,
        public int $teacherId,
        public int $harga
    ) {}

    public static function fromRequest($request): self
    {
        // Kita berani ambil data langsung karena sudah divalidasi FormRequest sebelumnya
        return new self(
            name: $request->validated('name'),
            teacherId: (int) $request->validated('teacher_id'), // Casting ke Int
            harga: (int) $request->validated('harga')
        );
    }
}
```

#### Tahap 3: Controller (Jembatan)

Menghubungkan keduanya.

```php
public function store(StoreKelasRequest $request) // 1. FormRequest Validasi dulu
{
    // 2. Jika lolos, code masuk sini.

    // 3. Konversi ke DTO
    $dto = KelasDTO::fromRequest($request);

    // 4. Kirim DTO ke Service
    $this->kelasService->create($dto);

    return response()->json(['message' => 'Sukses']);
}
```


#### Tahap 4: Service (Murni Bisnis)

Service tidak perlu validasi "apakah name kosong?" atau "apakah teacher ada?", karena dia percaya pada sistem sebelumnya.

```php
public function create(KelasDTO $data) // Menerima DTO
{
    // Enak banget coding di sini, ada auto-complete ($data->name)
    // Tidak perlu if-else validasi input lagi.
    return Kelas::create([
        'name' => $data->name,
        'teacher_id' => $data->teacherId,
        'harga' => $data->harga
    ]);
}
```


### Kesimpulan: Apakah DTO Bisa Validasi?

Bisa, tapi **tidak disarankan** untuk validasi input form (seperti  *required* ,  *email* ,  *unique* ).

* **Gunakan FormRequest untuk:** Validasi input dari User/Front-end (Format email, unique DB, required, max length).
* **Gunakan DTO untuk:** Memastikan **Tipe Data** (Type Casting) dan struktur data antar layer aplikasi.
