# Laravel Query

ini kumpulan-kumpulan case yang saya alami selama mengerjakan projet berikut beberapa solve dari permasalahan saya:

###### cek data field kosong spesifik pada table :

```php
$isEmpty = DB::table('laundrys')
                ->whereJsonLength('location_now', '=', 0)
                ->exists(); //hasil return dari variabel ini adalah boolean
//maka kita bisa menggunakan variable ini untuk kondisi yang kita butuhkan
if($isEmpty) {
  // tulis sesuatu..
}else{
  // tulis sesuatu..
}
```

###### cara insert multiple record.

```php
public function addKurikulum ($kelas_id,$request)
    {
        $data =[];
        for ($i=0; $i < count($request->kurikulum); $i++) {
            # code...
            echo $i .$request->kurikulum[$i].'<br>';
            $data [] =[
                'kelas_id'=>$kelas_id,
                'kurikulum'=>$request->kurikulum[$i]
            ];
        }
        KurikulumModel::insert($data);
    }
public function updateKurikulum ($request)
    {
        /*
            dear develop sekolahskill after here..
            untuk kasus update multipel record yang hanya mengandalkan single column table disarankan melakukan
            rselove seperti di bawah ini,walaupun ini belum terbukti work it untuk di setiap project, tapi
            ini sudah lebih dari cukup,
            walaupun resikonya adalah bertambahnya index dari database itu sendiri tapi bisa di pastikan beberapa
            aplikasi tidak akan sering melakukan perubahan besar secara terus menerus untuk kasus multple recoerd seperti kasus
            ini..
        */

        $data=[];
        KurikulumModel::where('kelas_id',$request->kelas_id)
                    ->delete();

        for($i=0;$i <count($request->kurikulum); $i++){
            $data [] =[
                'kelas_id'=>$request->kelas_id,
                'kurikulum'=>$request->kurikulum[$i]
            ];
        }
        KurikulumModel::insert($data);
    }
```

###### cara softdelete()

Menggunakan Soft Deletes di Laravel memungkinkan Anda "menghapus" data dari database tanpa benar-benar menghapusnya secara fisik, sehingga data tersebut masih dapat dipulihkan jika diperlukan. Berikut langkah-langkahnya:

**Langkah 1: Membuat Kolom Soft Deletes di Tabel**

Pertama, pastikan tabel Anda memiliki kolom `deleted_at` yang diperlukan untuk menyimpan informasi tanggal dan waktu ketika data dihapus secara "lunak". Anda dapat menambahkan kolom ini dengan menggunakan migrasi:

```bash
php artisan make:migration add_deleted_at_to_table_name
```

Buka file migrasi yang baru dibuat di direktori `database/migrations` dan tambahkan kode berikut di dalam method `up()`:

```php
public function up()
{
    Schema::table('table_name', function (Blueprint $table) {
        $table->softDeletes();
    });
}
```

Jalankan migrasi dengan perintah `php artisan migrate` untuk menambahkan kolom `deleted_at` ke tabel.

**Langkah 2: Menggunakan Soft Deletes di Model**

Di model yang sesuai, Anda perlu menggunakan trait `SoftDeletes` untuk mengaktifkan perilaku soft deletes. Buka model yang terkait dan tambahkan trait ini di sana:

```php
use Illuminate\Database\Eloquent\SoftDeletes;

class YourModel extends Model
{
    use SoftDeletes;

    // ...
}
```

**Langkah 3: Melakukan Soft Delete**

Anda dapat melakukan soft delete pada sebuah instance model dengan menggunakan method `delete()`. Contoh:

```php
$record = YourModel::find($id);
$record->delete();
```

**Langkah 4: Memulihkan Data yang Dihapus**

Anda dapat memulihkan data yang dihapus secara lunak dengan menggunakan method `restore()`. Contoh:

```php
$record = YourModel::withTrashed()->find($id);
$record->restore();
```

**Langkah 5: Menampilkan Data yang Dihapus**

Untuk menampilkan data yang telah dihapus (tetapi belum di-restore), Anda bisa menggunakan method `withTrashed()`:

```php
$records = YourModel::withTrashed()->get();
```

**Langkah 6: Menampilkan Hanya Data yang Dihapus**

Anda juga bisa menampilkan hanya data yang telah dihapus tanpa data yang tidak dihapus:

```php
$records = YourModel::onlyTrashed()->get();
```

**Langkah 7: Menghapus Data Permanen**

Jika Anda ingin menghapus data secara permanen, Anda dapat menggunakan method `forceDelete()`:

```php
$record = YourModel::withTrashed()->find($id);
$record->forceDelete();
```

Dengan langkah-langkah di atas, Anda dapat menggunakan fitur soft deletes di Laravel untuk mengelola penghapusan data secara lebih aman dan reversible. Ingatlah bahwa fitur soft deletes hanya berlaku jika Anda menggunakan model Eloquent untuk mengakses data.

###### Relatioan belongsto dan hasMany:

Ya, Anda perlu menuliskan relasi `hasMany` pada model `CategorySample` jika Anda sudah menulis relasi `belongsTo` pada model `Parameter`. Ini adalah bagian penting dari konsep relasi dalam Eloquent di Laravel.

Relasi `belongsTo` digunakan untuk menunjukkan bahwa model saat ini "dimiliki oleh" model lain. Ini berarti bahwa dalam model `Parameter`, Anda menyatakan bahwa setiap instance `Parameter` memiliki relasi dengan satu `CategorySample` (sebagai pemiliknya).

Namun, jika Anda ingin mengakses semua parameter yang terkait dengan suatu `CategorySample`, Anda harus menentukan relasi `hasMany` pada model `CategorySample`. Ini akan memungkinkan Anda mengambil semua instance `Parameter` yang terkait dengan satu `CategorySample`.

Jadi, untuk contoh Anda, penulisan yang Anda sebutkan adalah benar:

**Model Parameter:**

```php
class Parameter extends Model
{
    public function categorySample()
    {
        return $this->belongsTo(CategorySample::class, 'category_sample', 'id');
    }
}
```

**Model CategorySample:**

```php
class CategorySample extends Model
{
    public function relatedParameters()
    {
        return $this->hasMany(Parameter::class, 'parameter', 'id_category_sample');
    }
}
```

Dengan adanya relasi `hasMany` pada `CategorySample`, Anda akan dapat mengambil semua instance `Parameter` yang terkait dengan satu `CategorySample`. Ini memungkinkan Anda melakukan operasi yang lebih kompleks dan fleksibel terkait dengan hubungan antara model-model tersebut.

###### handle error di query yang berelasi dengan try-cath:

Di dalam sebuah transaksi menggunakan `DB::transaction()`, Anda tidak perlu menuliskan blok `try-catch` untuk mengatur commit atau rollback. Jika ada pengecualian (exception) dilemparkan dalam transaksi, Laravel secara otomatis akan melakukan rollback transaksi. Jika tidak ada pengecualian, maka transaksi akan di-commit secara otomatis.

Namun, jika Anda ingin menangkap pengecualian dan melakukan tindakan tertentu berdasarkan jenis pengecualian yang dilemparkan, Anda masih dapat menambahkan blok `try-catch` di dalam transaksi. Contoh:

```php
use Illuminate\Support\Facades\DB;
use Illuminate\Database\QueryException;

try {
    DB::transaction(function () use ($userId, $newAddress) {
        $user = User::find($userId);
        $profile = $user->profile;

        if ($profile) {
            $user->update([
                // Update fields in User model
            ]);

            $profile->update([
                'address' => $newAddress,
                // Update fields in Profile model
            ]);
        }
    });
} catch (QueryException $e) {
    // Tangani pengecualian khusus dari query yang gagal
    // Misalnya, lakukan log atau tindakan lain
} catch (\Exception $e) {
    // Tangani pengecualian umum
    // Misalnya, lakukan log atau tindakan lain
}
```

Jika Anda hanya ingin menggunakan commit dan rollback default Laravel, Anda tidak perlu menambahkan blok `try-catch`. Laravel akan mengelola transaksi dan rollback secara otomatis jika pengecualian terjadi.

Namun, jika Anda ingin mengambil tindakan tertentu tergantung pada jenis pengecualian yang dilemparkan (seperti menangani QueryException secara berbeda dari Exception umum), maka Anda dapat menambahkan blok `try-catch` seperti contoh di atas.
