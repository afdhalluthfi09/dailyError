# PHPMAKER
 ### Cara Costume Auto Update Value 
 Kasus : Ketika Ada Tabel Yang butuh Value tetap untuk inputan datanya
 - Pergi Ke tabel yang dituju 
 - lalu klik tab server event,client server
 - cari global -> code global lalu ketikan buat function perintah yang akan dijadikan auto update value :
 ```PHP
    //nama function bebas
    function auto_update_value()
    {
        // query
        $sql ='SELECT * FROM name_table where id = $sesuaiyangdicustome';
        $row = ExecuteRow($sql);
        return $row[0];
    }
```
- setelah melakukan custome function, pergi ke tools->advance settings->auto_update_value, lalu daftarkan function dibuat tadi
- lalu klik tabel yang akan di isi auto_update_value klik kolom yang di inginkan terus cari tab yang bertuliskan auto_update_value dan cari function yang sudah didaftarkan tadi.

    