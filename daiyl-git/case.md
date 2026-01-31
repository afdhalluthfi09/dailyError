# Selective Checkout.
Berikut langkah-langkah teknis agar aman dan sesuai ekspektasi Anda:

## 1. Metode Paling Aman: git checkout (Ambil File Tertentu)
Ini adalah cara terbaik untuk kasus Anda. Anda sedang berada di main, lalu Anda "mencomot" file tertentu dari branch lain tanpa membawa history commit yang berantakan atau file sampah lainnya.

Langkahnya:
### 2. Pindah ke branch main (Target):
git checkout main
### 3. Tarik file spesifik dari branch project (Sumber): Gunakan format: git checkout [nama_branch_sumber] -- [path/ke/file]
ex: git checkout project-sekolah -- src/Helpers/UserHelper.php
<br>Catatan: Anda bisa mengambil beberapa file sekaligus dengan spasi, atau satu folder full.

### 4. Cek status (Verifikasi): File tersebut sekarang sudah ada di main dan statusnya "staged" (siap di-commit).
git status

### 5. Commit dan Push:
git commit -m "Update core logic: UserHelper from project-sekolah" <br>
git push origin main