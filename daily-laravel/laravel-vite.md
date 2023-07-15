# Daiyl Laravel-Vite

ini catatan kecil mengenai konfigurasi dan beberapa solusi yang nantinya solusi ini bisa berubah kapan saja tergantung dari pengalaman dan keadaan tiap kasus yang di temui

###### installasi vite

* lakukan installasi awal dengan cara `npm install`

###### cara mengambil value .env di laravel vite `<br>`

file .env

```env
VITE_URL=https://example.com
```

maka cara memanggilnya di javascript cukup manggil dengan keyword
"import.meta.env.VARIABLE_ENV_VITE" agar bisa di panggil dalam konfigurasi vite usahakan nama variable di mulai dengan kata vite:

```javascript
 let url =import.meta.env.VITE_URL
```
