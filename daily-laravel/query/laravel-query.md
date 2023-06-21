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
