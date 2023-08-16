# rLaravel-Cros

kasus yang saya alami ini adalah terjadinya cros perizinan dari permintaan client server ke backend (server restApi) dimana client tidak bisa menambil data di karenakan header data yang di ada di respon api tidak ketahui oleh pihak client bentuk header restApi:

```json
asyn function fetchData()
{
let header ={
  "Access-Control-Allow-Origin": "*",
 await fetch('urlEndPoint',{
	method:TypeMethod,
	header:header
    })
    .then((respon)=>{respon.json})
    .then((data)=>{data.data})
    .catch((error)=>{console.log(error))})
}
```

pada saat kasus ini terjadi,saya menggunakan laravel sebagai base framework project,disana ada perubahan regulasi saat melakukan permintaan data dari sisi client kita harus melakukan memasang package cros di framework yang kita gunakan, dan ada dua solusi saat terjadi cros-origin:

* melakukan fetch data dari proses belakang:

  * yaitu meminta data dari `controller` disana kita menggunakan builder function http untuk fecth data

    ```php
    classs YourNameController extends Controller
    {
    	public function fecthDataExamp(){
    		$data = htpp::get(urlEnpoint);
    		return $response = $data->json;
    	}
    }
    ```

    lalu lakukan parsing ke dalam blade yang sudah kita siapkan
* atau install package `"fruitcake/laravel-cors": "^3.0"`###### * dengan menulis command `composer require yourPackgeComposer`
* daftarkan package middleware di `app\Http\Kernel.php` cari variable $middleware

  ```php
  protected $middleware = [
      // ...
      \Fruitcake\Cors\HandleCors::class,
  ];
  ```
* dan daftarkan cros ke publish dengan menulis di command `php artisan vendor:publish --tag="cors"`.
* bukan file `config/cors.php` jika berhasil nanti filenya sudah tergenarate seperti dibawah ini:

  ```php
  'paths' => ['api/*'],

  'allowed_methods' => ['*'],

  'allowed_origins' => ['http://your-domain.com'],

  'allowed_origins_patterns' => [],

  'allowed_headers' => ['*'],

  'exposed_headers' => [],

  'max_age' => 0,

  'supports_credentials' => false,

  ```

  jika sudah maka  lakukan `php artisan cache:clear` dan silahkan menikmati perpusingan berikutnya.

  ##### Cors Origin Blocked.
* ```javascript
   "Access to fetch at 'https://api.e-laundry.site/api/v1/data-order-p/117' from origin 'https://admin.e-laundry.site' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource".
  ```

  ```javascript
  Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://api.e-laundry.site/api/v1/data-order-p/118. (Reason: CORS header ‘Access-Control-Allow-Origin’ missing). Status code: 500.
  ```

  Pesan kesalahan yang Anda temui menunjukkan bahwa permintaan lintas domain (Cross-Origin Request) diblokir karena kurangnya header CORS 'Access-Control-Allow-Origin' pada sumber daya jarak jauh (https://api.e-laundry.site/api/v1/data-order-p/118). Status code 500 menandakan bahwa ada kesalahan internal di server.

  Untuk mengatasi masalah ini, pastikan Anda telah mengikuti langkah-langkah berikut:

  1. Pastikan Middleware CORS Terdaftar:
     Pastikan Anda sudah mendaftarkan middleware CORS pada file `app/Http/Kernel.php` di bagian `$middleware` dan `$middlewareGroups`, seperti yang telah dijelaskan sebelumnya.
  2. Periksa Konfigurasi CORS:
     Pastikan file konfigurasi CORS `config/cors.php` telah diatur dengan benar, terutama bagian `allowed_origins`. Pastikan bahwa domain situs aplikasi Anda yang sebenarnya terdaftar dalam array `allowed_origins`. Misalnya, jika Anda ingin mengizinkan akses dari situs aplikasi `https://admin.e-laundry.site`, pastikan Anda telah menambahkan domain tersebut ke dalam `allowed_origins` seperti berikut:

     ```php
     // config/cors.php

     return [
         'allowed_origins' => ['https://admin.e-laundry.site'],
         // ...
     ];
     ```
  3. Periksa Server API:
     Pastikan server API Anda (https://api.e-laundry.site) telah diatur untuk mengizinkan permintaan lintas domain dengan mengirimkan header `Access-Control-Allow-Origin` yang sesuai dalam responsnya. Pastikan bahwa Anda telah mengaktifkan middleware CORS di aplikasi Laravel API Anda dan mengatur konfigurasi CORS dengan benar, seperti yang telah dijelaskan pada langkah sebelumnya.
  4. Periksa Status Code 500:
     Status code 500 menandakan bahwa ada kesalahan internal di server API Anda. Periksa log kesalahan di server API untuk mendapatkan informasi lebih lanjut tentang kesalahan ini dan coba perbaiki masalahnya.

  Setelah melakukan langkah-langkah di atas, periksa kembali akses ke API dari situs aplikasi Anda. Pastikan bahwa header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API dan tidak ada lagi pesan kesalahan terkait CORS. Jika masih ada masalah, periksa log kesalahan di server API dan pastikan semua konfigurasi CORS sudah benar.

  ```javascript
  :Authority: api.e-laundry.site
  :Method: POST
  :Path: /api/v1/data-order-p/118
  :Scheme: https
  Accept: application/json, text/javascript, */*; q=0.01
  Accept-Encoding: gzip, deflate, br
  Accept-Language: en-US,en;q=0.9
  Authorization: Bearer 261|Iv7Wn3F6tVBWYYm3ykttSbACDLBxo5IkPU4DYXFp
  Content-Length: 71318
  Content-Type: multipart/form-data; boundary=----WebKitFormBoundarytUFe7GoJJ9BMl78C
  Origin: https://admin.e-laundry.site
  Referer: https://admin.e-laundry.site/
  Sec-Ch-Ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"
  Sec-Ch-Ua-Mobile: ?0
  Sec-Ch-Ua-Platform: "Linux"
  Sec-Fetch-Dest: empty
  Sec-Fetch-Mode: cors
  Sec-Fetch-Site: same-site
  ```

  ###### Informasi yang Anda berikan adalah header dan informasi permintaan (request) yang dikirim dari aplikasi dengan domain `admin.e-laundry.site` saat melakukan POST request ke endpoint `https://api.e-laundry.site/api/v1/data-order-p/118`.

  Pada header tersebut, beberapa informasi penting yang perlu diperhatikan adalah:


  1. `:Method: POST`: Ini menunjukkan bahwa permintaan yang dikirim adalah jenis HTTP POST request.
  2. `:Path: /api/v1/data-order-p/118`: Ini menunjukkan path atau alamat endpoint yang dituju dalam permintaan POST.
  3. `:Scheme: https`: Ini menunjukkan bahwa permintaan dikirim melalui protokol HTTPS, yang berarti permintaan dilakukan dengan aman melalui koneksi terenkripsi.
  4. `Authorization: Bearer 261|Iv7Wn3F6tVBWYYm3ykttSbACDLBxo5IkPU4DYXFp`: Ini menunjukkan bahwa ada header Authorization yang berisi token akses dengan metode Bearer, yang berarti aplikasi mengirimkan token akses ke server untuk mengautentikasi permintaan.
  5. `Content-Type: multipart/form-data; boundary=----WebKitFormBoundarytUFe7GoJJ9BMl78C`: Ini menunjukkan bahwa permintaan menggunakan tipe media multipart/form-data, yang biasanya digunakan untuk mengunggah file atau data yang kompleks.
  6. `Origin: https://admin.e-laundry.site`: Ini menunjukkan bahwa permintaan berasal dari domain `https://admin.e-laundry.site`, yang merupakan asal domain (origin) dari permintaan. Header ini diperlukan dalam mekanisme Same-Origin Policy dan CORS untuk mengontrol akses dari sumber daya pada domain lain.
  7. `Sec-Fetch-Mode: cors` dan `Sec-Fetch-Site: same-site`: Ini menunjukkan bahwa permintaan ini termasuk dalam kategori CORS (Cross-Origin Resource Sharing) dan memiliki mode "cors", yang berarti permintaan ini melibatkan akses lintas domain. Header ini berhubungan dengan mekanisme keamanan yang dilakukan oleh browser untuk mengontrol akses lintas domain.

  Dari informasi yang diberikan, secara umum, permintaan POST tersebut memiliki header yang standar dan tampaknya berjalan dengan benar. Jika ada masalah atau error yang Anda temui, pastikan bahwa konfigurasi CORS pada server API (`api.e-laundry.site`) sudah diatur dengan benar dan sesuai dengan domain yang diperbolehkan untuk mengaksesnya dari sumber domain lain seperti `admin.e-laundry.site`. Jika ada kesalahan atau blokade CORS, pastikan bahwa konfigurasi CORS sudah diatur dengan benar dan sumber daya pada server API sudah mengizinkan akses dari domain `admin.e-laundry.site`.
* 

###### memastika bahwa header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API.

Untuk memastikan bahwa header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API, Anda dapat melakukan langkah-langkah berikut:

1. Pastikan Middleware CORS Aktif:
   Pastikan Anda telah mendaftarkan middleware CORS di aplikasi Laravel Anda dan mengaktifkannya dengan benar. Anda dapat melakukan ini dengan mendaftarkan middleware CORS dalam file `app/Http/Kernel.php` di bagian `$middleware` atau `$middlewareGroups`.
2. Periksa Konfigurasi CORS:
   Pastikan file konfigurasi CORS `config/cors.php` di server API telah diatur dengan benar. Pastikan bahwa domain situs aplikasi Anda telah ditambahkan ke dalam array `allowed_origins` atau `allowed_origins_patterns` sesuai dengan kebutuhan.
3. Verifikasi Respons dari Server API:
   Untuk memeriksa apakah header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API, Anda dapat menggunakan alat pengembangan web di browser (misalnya, Chrome DevTools). Buka tab "Network" dalam alat pengembangan web, lalu lakukan permintaan ke API dan periksa responsnya. Di bagian "Response Headers" dalam tab "Headers", pastikan bahwa ada header dengan nama 'Access-Control-Allow-Origin' yang berisi nilai yang sesuai dengan domain situs aplikasi Anda.
4. Verifikasi dengan Fetch atau AJAX:
   Jika Anda melakukan permintaan ke API menggunakan Fetch atau AJAX di sisi klien (misalnya, dari aplikasi Vue.js atau JavaScript terpisah), pastikan Anda telah menambahkan atribut `mode: 'cors'` pada permintaan Anda. Ini akan memastikan bahwa permintaan dikirim dengan menggunakan mode CORS dan header CORS yang diperlukan akan diproses dengan benar oleh browser.

Contoh penggunaan Fetch dengan mode CORS:

```javascript
fetch('https://api.e-laundry.site/api/v1/data-order-p/118', {
    method: 'GET',
    mode: 'cors', // Pastikan mode cors diatur
    headers: {
        // Tambahkan header lain yang diperlukan
    }
})
.then(response => response.json())
.then(data => {
    // Proses data dari respons
})
.catch(error => {
    // Tangani kesalahan jika ada
});
```

Dengan melakukan langkah-langkah di atas, Anda dapat memastikan bahwa header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API. Jika setelah melakukan langkah-langkah tersebut masalah masih terjadi, periksa log kesalahan di server API dan pastikan bahwa konfigurasi CORS di server telah diatur dengan benar.

###### memeriksa apakah header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API, Anda dapat menggunakan alat pengembangan web di browser (misalnya, Chrome DevTools). Buka tab "Network" dalam alat pengembangan web, lalu lakukan permintaan ke API dan periksa responsnya. Di bagian "Response Headers" dalam tab "Headers", pastikan bahwa ada header dengan nama 'Access-Control-Allow-Origin' yang berisi nilai yang sesuai dengan domain situs aplikasi Anda.

Jika Anda sudah memverifikasi bahwa header CORS 'Access-Control-Allow-Origin' sudah ada dalam respons dari server API dan nilai header tersebut sesuai dengan domain situs aplikasi Anda, tetapi Anda masih mendapatkan pesan error "CORS blocked", berikut adalah beberapa langkah tambahan yang dapat Anda coba untuk menyelesaikan masalah ini:

1. Periksa Konfigurasi Middleware CORS: Pastikan Anda telah mendaftarkan middleware CORS dengan benar di aplikasi Laravel Anda. Periksa kembali apakah konfigurasi middleware CORS Anda telah diaktifkan dengan benar, terutama jika Anda menggunakan package "fruitcake/laravel-cors" atau package CORS lainnya. Pastikan bahwa konfigurasi CORS telah diaplikasikan dengan benar pada endpoint yang ingin Anda akses.
2. Periksa Metode Permintaan: Beberapa permintaan khusus, seperti permintaan dengan metode "OPTIONS", dapat mempengaruhi cara server API menangani permintaan CORS. Pastikan bahwa server API Anda telah dikonfigurasi dengan benar untuk menangani permintaan CORS yang berbeda, termasuk permintaan pra-flighted "OPTIONS" jika diperlukan.
3. Periksa Header Lainnya: Selain header "Access-Control-Allow-Origin", pastikan Anda juga telah mengatasi header CORS lainnya yang mungkin diperlukan oleh permintaan Anda, seperti "Access-Control-Allow-Headers", "Access-Control-Allow-Methods", dan lainnya. Pastikan konfigurasi header CORS di server API Anda sesuai dengan kebutuhan permintaan Anda.
4. Cek Log Kesalahan: Periksa log kesalahan di server API Anda untuk mencari tahu apakah ada kesalahan atau masalah lain yang menyebabkan permintaan CORS diblokir. Log kesalahan dapat memberikan petunjuk lebih lanjut tentang sumber masalah.
5. Nonaktifkan Ekstensi Browser: Beberapa ekstensi browser atau pengaturan keamanan pada browser dapat mempengaruhi cara browser menangani permintaan CORS. Coba nonaktifkan sementara ekstensi atau pengaturan keamanan yang mungkin mempengaruhi permintaan CORS Anda dan periksa apakah masalahnya masih ada.
6. Nonaktifkan Keamanan Sementara: Untuk tujuan uji coba, Anda dapat mencoba menonaktifkan sementara keamanan CORS pada server API Anda. Namun, ini harus dilakukan dengan hati-hati dan hanya untuk uji coba, karena menonaktifkan keamanan CORS dapat meningkatkan risiko keamanan pada server Anda.

Jika setelah mencoba langkah-langkah di atas masalah masih terjadi, pertimbangkan untuk memeriksa ulang konfigurasi server API Anda, termasuk middleware CORS dan konfigurasi header CORS, serta pastikan bahwa server API Anda telah dikonfigurasi dengan benar untuk menangani permintaan CORS dari domain situs aplikasi Anda. Jika Anda masih mengalami kesulitan, pertimbangkan untuk mencari bantuan dari komunitas atau forum pengembang Laravel yang lebih luas, karena mereka mungkin dapat memberikan saran lebih spesifik berdasarkan detail dan lingkungan aplikasi Anda.

###### script response api yang jika terjadi code 422 maka buatkan pesan dari log.

Untuk membuat script response API yang menangani kode status 422 dan mencatat pesan error dalam log, Anda dapat menggunakan middleware di Laravel. Berikut adalah contoh script middleware untuk menangani kasus tersebut:

1. Buat middleware baru dengan perintah artisan:

```bash
php artisan make:middleware Log422Errors
```

2. Buka file middleware yang baru dibuat di direktori `app/Http/Middleware/Log422Errors.php` dan ubah isinya seperti berikut:

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\Log;
use Illuminate\Http\JsonResponse;

class Log422Errors
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);

        // Cek apakah response merupakan instance dari JsonResponse dan memiliki kode status 422
        if ($response instanceof JsonResponse && $response->getStatusCode() == 422) {
            // Ambil pesan error dari response
            $error = $response->getData()->error ?? null;
            $message = $response->getData()->message ?? 'Unprocessable Entity';

            // Tulis pesan error ke log
            Log::error("422 Unprocessable Entity: $message", ['error' => $error]);
        }

        return $response;
    }
}
```

3. Daftarkan middleware baru yang telah dibuat di dalam file `app/Http/Kernel.php`. Tambahkan kode berikut di dalam array `$middleware`:

```php
protected $middleware = [
    // Middleware lainnya...
    \App\Http\Middleware\Log422Errors::class,
];
```

Setelah Anda membuat dan mendaftarkan middleware tersebut, middleware akan menangani setiap permintaan API yang menghasilkan kode status 422 dan mencatat pesan error tersebut ke dalam log.

Pastikan Anda telah mengatur logging di file `config/logging.php` sesuai dengan preferensi Anda, seperti mengatur jenis log yang ingin digunakan (file log, log database, dll.) dan level logging (debug, error, dll.). Pesan error akan dicatat sesuai dengan konfigurasi log yang Anda tentukan.

###### kesalahan mencamtumkan header content-type "Content-Type":["multipart/form-data","application/json"].

Masalah yang Anda hadapi mungkin terkait penggunaan Content-Type dalam header yang dikirimkan. Anda mencoba untuk mengirimkan formData, tetapi Anda juga mencantumkan `"Content-Type":["multipart/form-data","application/json"]` dalam header.

Jika Anda ingin mengirimkan formData, Anda seharusnya hanya menggunakan `"Content-Type": "multipart/form-data"` dalam header, tanpa mencantumkan `"application/json"`. Pengaturan header Anda harus terlihat seperti ini:

```javascript
let headersList = {
    'X-CSRF-TOKEN': csrfToken,
    "Accept": "application/json",
    "Content-Type": "multipart/form-data",
    "Authorization":`Bearer ${csrf.data('session')}`
}
```

Pastikan Anda menghapus `"application/json"` dari header, sehingga formData dapat dikirim dengan benar ke server. Setelah mengubah header, coba lagi untuk mengirimkan formData dan lihat apakah nilainya berhasil sampai ke controller.

Jika setelah menghapus `"application/json"` dari header dan masih tidak dapat mengirimkan data formData ke controller, berikut beberapa langkah yang dapat Anda coba:

1. Pastikan Anda mendefinisikan `enctype="multipart/form-data"` dalam elemen form Anda. Hal ini diperlukan untuk mengizinkan pengiriman data formData secara benar.
2. Periksa apakah atribut `name` dari elemen input dalam form telah didefinisikan dengan benar. Pastikan nama atributnya sesuai dengan apa yang diharapkan oleh controller Anda.
3. Di controller, pastikan Anda menggunakan metode `POST` untuk menangani data formData. Juga, pastikan Anda telah memeriksa dan memproses data dengan benar di sisi server.
4. Cobalah untuk mencetak atau log data formData sebelum mengirimnya ke server untuk memastikan bahwa data yang dikirimkan sesuai dengan yang Anda harapkan.

Jika Anda sudah memeriksa semua langkah di atas dan masih mengalami masalah, Anda mungkin perlu memeriksa log server atau pesan kesalahan untuk mencari tahu penyebab masalah tersebut. Jika ada pesan kesalahan atau log yang muncul, periksa pesan tersebut untuk mendapatkan petunjuk tentang apa yang salah dan bagaimana cara memperbaikinya. Jika Anda tidak melihat pesan kesalahan atau log, periksa apakah server menerima permintaan Anda dan coba untuk melakukan debugging lebih lanjut untuk mengetahui di mana masalahnya.

###### apkaah bisa fetch mengirim data dengan content-type multipart/form-data, jika bisa,  berikan model kodingan yang valid untuk kasus fecth.

Ya, fetch dapat mengirim data dengan content-type `multipart/form-data` menggunakan `FormData`. Berikut adalah contoh kode yang valid untuk mengirim data menggunakan fetch dengan content-type `multipart/form-data`:

```javascript
// Function untuk mengirim data menggunakan fetch
async function sendData() {
  let formData = new FormData();
  formData.append('name', 'John Doe');
  formData.append('email', 'john.doe@example.com');
  formData.append('avatar', document.querySelector('#avatar').files[0]);

  let response = await fetch('https://example.com/api/endpoint', {
    method: 'POST',
    body: formData,
  });

  let data = await response.json();
  console.log(data);
}

// Panggil function untuk mengirim data
sendData();
```

Dalam contoh di atas, kita membuat objek `FormData` dan menambahkan data ke dalamnya menggunakan metode `append()`. Data yang ditambahkan akan dikirimkan sebagai `multipart/form-data` dalam permintaan fetch.

Pastikan untuk mengganti URL `https://example.com/api/endpoint` dengan URL yang sesuai untuk endpoint Anda.

Perlu diingat bahwa server harus dikonfigurasi untuk dapat meng-handle permintaan dengan content-type `multipart/form-data`, dan data yang dikirimkan harus diolah dengan benar di sisi server.

###### kenapa fecth tidak perlu memasamng header unutk memberi tahu kalo data yang di terima content-tyepnya multipart/form-data,kapan kita harus menulis header?

Fetch tidak perlu secara eksplisit menetapkan header `Content-Type` ketika menggunakan `FormData`, karena ketika Anda menggunakan objek `FormData`, browser secara otomatis akan menetapkan header `Content-Type` ke `multipart/form-data` secara otomatis. Ini adalah salah satu fitur dari `FormData` yang mempermudah kita dalam mengirim data dengan tipe `multipart/form-data` melalui fetch.

Anda perlu menulis header secara eksplisit ketika Anda mengirim data dalam format selain `multipart/form-data`, seperti `application/json` atau `application/x-www-form-urlencoded`. Contoh penggunaan header saat mengirim data dalam format JSON adalah sebagai berikut:

```javascript
async function sendData() {
  let data = {
    name: 'John Doe',
    email: 'john.doe@example.com',
  };

  let response = await fetch('https://example.com/api/endpoint', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(data),
  });

  let responseData = await response.json();
  console.log(responseData);
}
```

Dalam contoh di atas, kita menetapkan header `'Content-Type': 'application/json'` untuk memberitahu server bahwa data yang dikirim dalam format JSON. Kita juga menggunakan `JSON.stringify(data)` untuk mengubah objek data menjadi bentuk JSON yang sesuai dengan body request.

Jadi, ketika Anda menggunakan `FormData` untuk mengirim data dengan tipe `multipart/form-data`, Anda tidak perlu menulis header secara eksplisit karena browser akan menanganinya untuk Anda. Namun, ketika Anda mengirim data dalam format lain, Anda perlu menulis header sesuai dengan tipe konten data yang dikirimkan.

###### mode no-cors.

mode permintaan ke `'no-cors'` sebagai opsi fetch Anda. Namun, ini hanya akan memungkinkan permintaan dilakukan, tetapi Anda tidak akan dapat mengakses data dari respons jika itu dianggap 'opaque' oleh browser. Misalnya:

```javascript
await fetch(`${import.meta.env.VITE_URL_LOKAL}/data-order-p/${idOrderP}`, {
    method: "post",
    body: formData,
    headers: headersList,
    mode: 'no-cors', // Tambahkan mode 'no-cors' ke opsi fetch
});

```

Harap dicatat bahwa menggunakan `mode: 'no-cors'` mungkin hanya cocok untuk beberapa kasus tertentu, terutama ketika Anda tidak perlu mengakses data dari respons. Jika Anda membutuhkan data dari respons, Anda perlu memperbaiki masalah CORS di server API agar header `Access-Control-Allow-Origin` disertakan dengan benar.

Ketika Anda menggunakan mode 'no-cors' pada permintaan fetch, Anda akan mengalami pembatasan dalam mengakses data respons dari server. Pada mode 'no-cors', Anda hanya dapat melakukan permintaan, tetapi Anda tidak dapat mengakses data respons menggunakan metode seperti `response.json()` karena konten dari respons tidak dapat diakses oleh JavaScript pada halaman web Anda.

Jika Anda perlu mengakses data respons dari server, Anda tidak dapat menggunakan mode 'no-cors'. Mode 'no-cors' digunakan untuk permintaan sederhana tanpa membaca data respons, seperti permintaan pesan log, atau untuk mengirimkan data ke server tanpa perlu membaca responsnya.

Jika Anda perlu mengakses data respons dari server, Anda harus menghapus mode 'no-cors' dari permintaan fetch Anda, sehingga permintaan bisa berjalan dengan mode standar (CORS aktif). Jika Anda mengalami masalah CORS, pastikan konfigurasi CORS pada server Anda sudah benar dan sesuai dengan domain yang Anda izinkan untuk mengakses API.

Anda juga dapat memeriksa log server dari API e-laundry.site untuk melihat apakah ada pesan atau kesalahan terkait dengan permintaan dari sisi admin.e-laundry.site. Ini dapat membantu Anda melacak masalah CORS dan mengetahui apakah server API telah menerima atau menolak permintaan dari domain admin.e-laundry.site dengan benar.

###### kenapa di untuk enpoint yang method get tidak mengalami cors blocked dari sisi client, hanya saat enpoint yang bertipe method post saja yang mengalami cors blocked.

Permasalahan CORS (Cross-Origin Resource Sharing) pada permintaan POST, tapi tidak pada permintaan GET, kemungkinan disebabkan oleh dua hal:

1. Konfigurasi CORS yang Tidak Sesuai: Ada kemungkinan bahwa pada sisi server, konfigurasi CORS belum tepat diatur untuk mengizinkan permintaan dari sumber yang berbeda (origin) saat menggunakan metode POST. Ini bisa terjadi jika konfigurasi CORS hanya diatur untuk mengizinkan permintaan dari sumber yang berbeda saat menggunakan metode GET, tetapi belum diatur untuk metode POST. Pastikan bahwa konfigurasi CORS sudah benar untuk kedua metode tersebut.
2. Content-Type: Pada permintaan POST, biasanya data dikirimkan dalam bentuk payload, seperti JSON atau form-data. Jika Anda menggunakan `Content-Type: application/json` atau `Content-Type: multipart/form-data`, permintaan akan menjadi "preflighted" oleh browser, artinya browser akan mengirimkan permintaan OPTIONS sebelum permintaan POST untuk memeriksa apakah server mendukung CORS dan mengizinkan permintaan dari sumber yang berbeda.

Jika server tidak memberikan respon dengan header CORS yang tepat, maka permintaan POST akan dianggap diblokir oleh browser. Untuk mengatasi ini, Anda harus memastikan bahwa server merespons permintaan OPTIONS dengan header CORS yang sesuai.

Untuk memastikan dan mengatasi masalah ini, pastikan bahwa konfigurasi CORS di sisi server sudah benar dan lengkap, serta menyertakan header CORS yang tepat pada respons untuk permintaan OPTIONS dan POST. Jika perlu, Anda dapat menggunakan middleware atau plugin khusus di framework Laravel untuk mengatur konfigurasi CORS secara lebih fleksibel dan sesuai kebutuhan aplikasi Anda.

###### kasus menggunakan subdomain.

Jika Anda menggunakan subdomain seperti dalam kasus Anda, di mana domain utama adalah e-laundry.site, tetapi Anda memiliki subdomain api.e-laundry.site dan admin.e-laundry.site, maka Anda masih harus mengatur header CORS dengan benar untuk memungkinkan permintaan dari subdomain yang berbeda.

Anda tidak dapat menggunakan wildcard ('*') untuk 'allowed_origins' jika Anda ingin mengatur izin secara spesifik untuk subdomain tertentu. Jika Anda ingin memungkinkan permintaan dari subdomain admin.e-laundry.site dan http://elaundry-admins.test, Anda harus menetapkan daftar domain yang tepat di 'allowed_origins' seperti yang Anda lakukan sebelumnya:

```php
'allowed_origins' => ['http://elaundry-admins.test', 'https://admin.e-laundry.site'],
```

Pastikan Anda juga telah memperbarui konfigurasi CORS di sisi admin.e-laundry.site untuk mengizinkan permintaan dari domain utama e-laundry.site dan subdomain api.e-laundry.site:

```php
'allowed_origins' => ['http://elaundry-admins.test', 'https://api.e-laundry.site'],
```

Dengan mengatur header CORS dengan benar di kedua sisi (api.e-laundry.site dan admin.e-laundry.site), Anda seharusnya dapat melakukan permintaan lintas domain antara domain utama dan subdomain. Pastikan Anda sudah mengonfigurasi file config/cors.php di kedua bagian tersebut dengan benar dan mengizinkan domain yang sesuai. Jika Anda masih mengalami masalah CORS, pastikan juga tidak ada kesalahan dalam konfigurasi server atau middleware lain yang mungkin mempengaruhi permintaan.
