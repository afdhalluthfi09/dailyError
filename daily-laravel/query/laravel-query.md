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
