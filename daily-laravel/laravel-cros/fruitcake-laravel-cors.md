# fruitcake/laravel-cors

###### Authorization headers / Credentials

If your Request includes an Authorization header or uses Credentials mode, set the supports_credentials value in the config to true. This will set the Access-Control-Allow-Credentials Header to true.

Dalam dokumentasi fruitcake/laravel-cors, bagian tersebut menjelaskan tentang cara mengatur pengaturan "credentials" atau kredensial pada permintaan CORS. "Credentials" dalam konteks CORS merujuk pada penggunaan cookie, header otorisasi (Authorization), atau header lain yang berisi informasi otentikasi.

Jika permintaan yang dikirimkan ke server API Anda mengandung header Authorization atau menggunakan mode "Credentials", Anda perlu mengatur nilai `supports_credentials` dalam konfigurasi CORS menjadi `true`. Dengan mengatur nilai ini menjadi `true`, maka Laravel akan menambahkan header Access-Control-Allow-Credentials dengan nilai true pada responsnya.

Dengan adanya header Access-Control-Allow-Credentials yang diatur ke true, browser akan mengizinkan penggunaan kredensial, seperti cookie, pada permintaan lintas domain (CORS). Ini memungkinkan pengiriman data otentikasi, seperti cookie otentikasi, dalam permintaan dari sumber yang berbeda.

Penting untuk diingat bahwa pengaturan ini juga perlu diakomodasi di sisi frontend (misalnya, menggunakan `withCredentials: true` pada fetch API JavaScript) agar browser mengirimkan kredensial bersamaan dengan permintaan.

Jadi, dengan mengatur `supports_credentials` menjadi `true`, Anda dapat memungkinkan akses ke sumber daya yang dilindungi oleh otentikasi atau menggunakan kredensial dalam permintaan CORS dari sumber yang berbeda.

###### Echo/die

If you use echo(), dd(), die(), exit(), dump() etc in your code, you will break the Middleware flow. When output is sent before headers, CORS cannot be added. When the script exits before the CORS middleware finishes, CORS headers will not be added. Always return a proper response or throw an Exception.

Maksud dari dokumentasi `fruitcake/laravel-cors` adalah bahwa ketika Anda menggunakan fungsi-fungsi seperti `echo()`, `dd()`, `die()`, `exit()`, `dump()` dan sejenisnya dalam kode Anda, Anda akan menghentikan aliran Middleware. Ketika output (seperti teks dari `echo()`) dikirimkan sebelum header, Middleware CORS tidak dapat menambahkan header CORS. Ketika skrip berhenti sebelum Middleware CORS selesai, header CORS tidak akan ditambahkan.

Untuk menghindari masalah ini, Anda disarankan untuk selalu mengembalikan respons yang benar atau melempar Exception dari controller atau middleware Anda, daripada menggunakan fungsi-fungsi seperti `echo()` atau `die()`. Dengan cara ini, aliran Middleware akan selesai secara normal, dan Middleware CORS dapat menambahkan header CORS yang diperlukan sebelum respons dikirimkan kembali ke klien.

Jadi, pastikan untuk selalu mengembalikan respons yang tepat atau melempar Exception di dalam kode Anda, dan hindari menggunakan fungsi-fungsi yang dapat menghentikan aliran Middleware sebelum header CORS ditambahkan.

###### No Cross-Site requests

If you are not doing Cross-Site requests, meaning if you are not requesting site-a.com/api from site-b.com, your browser will not send the Origin: https://site-b.com request header, CORS will be "disabled" as the Access-Control-Allow-Origin header will be also missing. This happens because requests are being dispatched from the same and no protection is needed in this case.

Pernyataan tersebut dari dokumentasi fruitcake/laravel-cors menjelaskan tentang cara kerja CORS (Cross-Origin Resource Sharing) dan kondisi di mana header CORS akan "dinonaktifkan" atau tidak ada.

CORS merupakan mekanisme keamanan yang diimplementasikan oleh browser untuk melindungi pengguna dari potensi ancaman keamanan saat mengakses sumber daya (resource) dari domain yang berbeda (origin). Ini berarti, ketika Anda melakukan permintaan (request) dari satu domain (misalnya site-b.com) ke domain lain (misalnya site-a.com) melalui JavaScript di browser, browser akan mengirimkan header "Origin" dengan nilai "https://site-b.com" untuk memberitahu server di site-a.com bahwa permintaan tersebut berasal dari domain site-b.com.

Namun, jika Anda tidak melakukan permintaan antar domain seperti di atas, misalnya hanya melakukan permintaan dari JavaScript di site-b.com ke situs yang sama site-b.com, maka browser tidak akan mengirimkan header "Origin". Dalam kondisi ini, CORS tidak diperlukan karena permintaan berlangsung di dalam satu domain, sehingga tidak ada perlindungan tambahan yang dibutuhkan.

Oleh karena itu, jika Anda tidak melakukan permintaan lintas domain (Cross-Site requests), header "Origin" tidak akan ada, dan CORS akan dinonaktifkan secara efektif karena header "Access-Control-Allow-Origin" juga tidak akan ada. Hal ini karena browser menganggap permintaan tersebut aman karena berlangsung di dalam satu domain, dan tidak memerlukan perlindungan tambahan dari CORS.
