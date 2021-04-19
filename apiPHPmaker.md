# Api PHPMAKER2020
cara menggunakan api phpmkaer yang harus diperhatikan:
- jika aplikasi menggunakan system login maka api yang dihasilkan akan meminta token sebelum request data di tampilkan.
- jika aplikasi tidak menggunakna system login, maka api yang dihasilkan tidak akan meminta token.
>jiak tidak menggunakan berier akan tetapi itu akan membahayakan aplikasi itu sendiri.
---
untuk menuliskan api dari phpmaker maka kita cukup menuliskan seperti ini
```php
//gambaran tulisan urlApi
namadomain/namaproject/api/action=nama_action&object=namatabel

//contoh kasus
http:localhost/phpmaker/api/?action=list&object=artikel
```
jika kita menulisakn action adalah list maka yang di tampilkan ada berupa hasil seluruh data yang ada pada tabel yang kita sudah buat
maka nanti hasil request seperti ini :
```json
{
    "success": true,
    "artikel": [
        {
            "id": "3",
            "gambar_judul": {
                "mimeType": "image/jpeg",
                "url": "https://gonihon.jp/files/figure-3956569_640.jpg"
            },
            "judul": "Sekilas tentang Jepang",
            "jenis_artikel": "1",
            "tanggal_post": "2021-04-18"
        },
        {
            "id": "4",
            "gambar_judul": {
                "mimeType": "image/jpeg",
                "url": "https://gonihon.jp/files/people-400818_640.jpg"
            },
            "judul": "Kekurangan Tenaga Kerja, Jepang Menerima TKA",
            "jenis_artikel": "1",
            "tanggal_post": "2021-04-18"
        }
}
```
jika kita menggunakna system login pada aplikasi kasih kita maka terlebih dahulu kita harus meminta token yang sudah di generate oleh system aplikasi 
```php
url : http:localhost/phpmaker/api/?action=login&usrname=admin&password=123Admin
```
maka request yang di hasilkan adalah sebuah token yang digenerate menggunakan pihak ketiga yaitu libarary JWT seperti dibawah ini:
```json
{
    "JWT": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE2MTg3ODg2ODksImp0aSI6Iml2QVpWMDk1TlZMVndZQk5meEJ2Zzh6SWU0NXJnMDYyUjk0MTFwQzJ4MjQ9IiwiaXNzIjoiZ29uaWhvbi5qcCIsIm5iZiI6MTYxODc4ODY4OSwiZXhwIjoxNjE4Nzg5Mjg5LCJzZWN1cml0eSI6eyJ1c2VybmFtZSI6Imdvbmlob24iLCJ1c2VyaWQiOm51bGwsInBhcmVudHVzZXJpZCI6bnVsbCwidXNlcmxldmVsaWQiOi0yfX0.v7T1XWUWZBKlYo26S4Ryp_DbolRWvxyBmKMYjVjTAWP-YGAkMkVodyXsQ3qNg-OorSOIwrjcswVsID239IZdyQ"
}
```
dan token ini harus dimasukan kedalam struktur <strong>headers</strong> dari api kita dengan format seperti ini :

```Api
X-Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpYXQiOjE2MTg3ODg2ODksImp0aSI6Iml2QVpWMDk1TlZMVndZQk5meEJ2Zzh6SWU0NXJnMDYyUjk0MTFwQzJ4MjQ9IiwiaXNzIjoiZ29uaWhvbi5qcCIsIm5iZiI6MTYxODc4ODY4OSwiZXhwIjoxNjE4Nzg5Mjg5LCJzZWN1cml0eSI6eyJ1c2VybmFtZSI6Imdvbmlob24iLCJ1c2VyaWQiOm51bGwsInBhcmVudHVzZXJpZCI6bnVsbCwidXNlcmxldmVsaWQiOi0yfX0.v7T1XWUWZBKlYo26S4Ryp_DbolRWvxyBmKMYjVjTAWP-YGAkMkVodyXsQ3qNg-OorSOIwrjcswVsID239IZdyQ
```
akan tetapi ada yang harus dipastikan beberapa hal dalam menggunakna api dari phpmaker ini 
> - pastikan waktu expaired sudah di ataur sesuai dengan kebutuhan bisa di akses melalui  tools -> advance settings ->  Api Expire time after login .
---
jika pemeberitahuan lebih dalam bisa baca di bagain help phpmaker.