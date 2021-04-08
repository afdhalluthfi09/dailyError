# PHPMAKER
### redirect url detail
kasus saat melakukan redirect saya sering kali melakukan redirect langsung ke datanya tanpa melakukan pengkombinasian data, sehingga orang lebih mudah dibaca dengan begitu data saya jadi tidak aman lah kan jadi cara terbaiknya seperti dibawa ini agar data kita tidak mudah di baca wong hecker:
>lakukan pencegahnaya dnegan menggunakan function yang bisa mengacak data kita saat dalam pengiriman seperti md5() base64_decode() dll.
---
contoh dalam url redirect kita tambhaan function di atas
```HTML
    <a href="domain.com/situs.php?b=<?=$r['alamat_id']?>"></a>
    <a href="domain.com/situs.php?b=<?=md5($r['alamat_id'])?>"></a>
```
hasilnya seperti berikut
> http://domain.com/situs.php?b=4 <br>
http://domain.com/situs.php?b=1281281128128
