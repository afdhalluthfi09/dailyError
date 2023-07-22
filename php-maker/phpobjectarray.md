# phpmaker v2020
jika mendapati suatu result dari array dengan bentukan seperti ini :
```php
str(80):{
    "lat":"-72.00909",
    "lang":"890889"
}
```
maka cara untuk mendaptakan setiap isinya cukup dengan menggunakan library php json_decode(); berikut contohnya:
```PHP
    $b =json_decode($JSON);
    $bj=$b->lat;
    $bj1=$b->long;
```