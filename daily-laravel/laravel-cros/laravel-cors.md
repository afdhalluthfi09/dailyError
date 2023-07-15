# Laravel-Cros

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
* atau install package `"fruitcake/laravel-cors": "^3.0"`
  * dengan menulis command `composer require yourPackgeComposer`
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
