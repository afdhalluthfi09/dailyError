# Laragon

###### installasi:

Instalasi Laragon di Windows 10 relatif sederhana. Laragon adalah lingkungan pengembangan web yang mudah digunakan dan terintegrasi. Berikut adalah langkah-langkah instalasi Laragon:

1. **Unduh Installer Laragon:**

   Kunjungi situs resmi Laragon di https://laragon.org/ dan unduh versi Laragon yang sesuai untuk Windows. Anda akan menemukan pilihan "Laragon" dan "Laragon Full". "Laragon Full" telah disertai dengan Apache, MySQL, PHP, dan banyak fitur lainnya. "Laragon" adalah versi dasar. Pilih sesuai kebutuhan Anda.
2. **Jalankan Installer:**

   Setelah unduhan selesai, jalankan file installer Laragon yang telah Anda unduh. Ikuti petunjuk yang muncul di layar.
3. **Pilih Lokasi Instalasi:**

   Pada langkah ini, Anda akan diminta untuk memilih lokasi di mana Anda ingin menginstal Laragon. Pilih lokasi yang sesuai dan klik "Next".
4. **Pilih Komponen yang Akan Diinstal:**

   Jika Anda memilih Laragon Full, Anda mungkin akan diminta untuk memilih komponen yang ingin Anda instal, seperti Apache, MySQL, PHP, dan lainnya. Pilih yang sesuai dan klik "Next".
5. **Pilih Tipe Instalasi:**

   Anda akan diminta untuk memilih tipe instalasi, apakah ingin instalasi standar atau kustom. Anda dapat memilih standar jika tidak yakin. Klik "Next".
6. **Tentukan Folder Dokumen Projek:**

   Anda akan diminta untuk menentukan folder di mana Anda akan menyimpan proyek-proyek Anda. Anda dapat menggunakan folder default atau menentukan folder lain. Klik "Next".
7. **Selesai Instalasi:**

   Setelah Anda mengkonfigurasi semua opsi, klik "Install" atau "Next" (tergantung pada langkah sebelumnya). Tunggu hingga proses instalasi selesai.
8. **Selesai!**

   Setelah instalasi selesai, Anda akan melihat pesan yang memberi tahu Anda bahwa instalasi telah berhasil. Anda sekarang dapat membuka Laragon dan mulai menggunakannya untuk mengembangkan aplikasi web Anda.
9. **Mulai Laragon:**

   Setelah instalasi selesai, Anda akan menemukan shortcut Laragon di desktop Anda atau di menu Start. Buka Laragon dengan mengklik shortcut tersebut.
10. **Mulai Server:**

    Dalam jendela Laragon, Anda akan melihat tombol "Start All" untuk memulai server Apache dan MySQL. Klik tombol ini untuk memulai server. Anda juga dapat memulai server secara individu jika diperlukan.
11. **Buka Proyek:**

    Anda dapat membuat atau mengimpor proyek Laravel, WordPress, atau proyek lainnya ke dalam folder dokumen projek yang Anda tentukan saat instalasi. Anda juga dapat membuat proyek baru melalui Laragon.
12. **Mengakses Proyek:**

    Setelah server dimulai dan proyek terkonfigurasi dengan benar, Anda dapat mengakses proyek Anda melalui browser dengan alamat `http://localhost/nama-proyek`.

Sekarang Anda telah berhasil menginstal Laragon di Windows 10 dan siap untuk memulai pengembangan web Anda. Pastikan Anda membaca dokumentasi resmi Laragon untuk memahami lebih dalam tentang fitur-fitur dan cara penggunaan yang lebih lanjut.

###### cara swicth version php.



Untuk beralih antara versi PHP di Laragon di Windows 10, ikuti langkah-langkah berikut:

1. **Buka Laragon:**

Pastikan Laragon sudah terinstal dan berjalan di sistem Anda. Anda akan melihat ikon Laragon di taskbar atau di sistem tray.

2. **Klik pada Nama PHP di Tray:**

Klik kanan pada ikon Laragon di taskbar atau tray (ikon yang biasanya menunjukkan huruf "L"). Setelah itu, Anda akan melihat daftar versi PHP yang tersedia.

3. **Pilih Versi PHP yang Diinginkan:**

Pilih versi PHP yang ingin Anda gunakan dari daftar yang muncul. Laragon akan memulai ulang server dan menggunakan versi PHP yang baru.

4. **Verifikasi Versi PHP:**

Untuk memastikan bahwa Laragon telah beralih ke versi PHP yang diinginkan, buka terminal Laragon (klik kanan pada ikon Laragon, lalu pilih "Terminal") dan ketik perintah berikut:

```bash
php -v
```

Perintah ini akan menampilkan versi PHP yang sedang aktif.

Selain itu, Anda juga dapat mengatur versi PHP default yang ingin digunakan Laragon dengan langkah berikut:

1. Klik kanan pada ikon Laragon di taskbar atau tray.
2. Pilih "Preferences" atau "Options" (tergantung pada versi Laragon yang Anda gunakan).
3. Di bagian "General", pilih versi PHP default yang ingin Anda gunakan dari menu dropdown.
4. Klik "Save" atau "Apply" untuk menyimpan pengaturan.

Laragon sekarang akan menggunakan versi PHP yang Anda atur sebagai default setiap kali Anda memulai server.

Pastikan Anda memiliki versi PHP yang diinginkan telah diinstal dan terdaftar di Laragon. Anda dapat menambahkan atau menghapus versi PHP melalui menu Laragon "Tools" > "Quick add" atau "Configuration" > "PHP".

Perlu diingat bahwa jika Anda memiliki proyek yang memerlukan versi PHP tertentu, Anda harus memastikan bahwa proyek tersebut sesuai dengan versi PHP yang sedang digunakan oleh Laragon.
