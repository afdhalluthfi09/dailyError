# Tips

## 🐱‍👓 Query SQL
Setiap Ingin Melakuakn Fetch Data Menggunakan MYSQL lebih baik selalu terapkan try & catch data agar bisa diketahui lebih cepat letak kesalahanya.<br>
>kasus: <br>
    Ketika Project kita sudah banyak mengakses banyak query atau pindah database terakdanga banyak terjadi kerusakan query yang disebakan perpidahan tersebut karena beberapa hal seperti versi dari database yang kita gunakan sebelumnya, konfigurasi yang digunakan saat di database lama belum di update di database yang baru dll.. sehingga ketika pemanggilan project berlangsung banyak yang harus diperbaiki untuk mengatasi itu semua salah satu jalanya adalah membuat pengecekan saat melakukan fetch query dengan begini kita bisa mengetahui bagian mana yang menjadi keselahan saat pemamnggilan data.

berikut cara melakukan pengeckanya 
```PHP
$host='yourhost';
$port=22;
$db_name='your db name';
$db_pass='your pass';
$db_user='yuor name user'

// buat koneksi
$mysqli = new mysqli($host,$db_user,$db_pass,$db_name);
$check1_task = "SELECT * FROM `users`;

