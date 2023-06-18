# Laravel-Scantum

tulisan ini hanya berisikan kesalahan-kesalahn saya saat mebuild beberapa backend beriku kesalahanya dan solvenya:

* saat melakuan generate token pada aplikasi backend guna bisa mendapatkan hak ases data yang di tuju, tapi saat melakan terdapat kendala pada gamabr di bawah ini:

  ![1687062068400](image/laravel-scantum/1687062068400.png)yang mana menimbulkan tanda merah pada code diatas dan mendapatkan pesan seperti ini : `Undefined method 'createToken'.`
* penyebanya karna saat kita inisialisasi variable `$user` maka yang dianggap adalah variable yang sama pada package Auth yang di package scantum, padahal yang di inginkan adalah variable `$user` yang ada pada model `App\Models\Users` maka dari itu kita kasih blok variabel seperti yang dibawah ini:
  `/**  @var \App\Models\User $user */`

  ```php
  if (Auth::attempt($credential)) {
              /**  @var \App\Models\User $user */
              $user = Auth::user();

              $adminToken = $user->createToken('admin-token', ['create','update','delete']);
              $updateToken = $user->createToken('update-token', ['create', 'update', 'delete']);
              $basicToken = $user->createToken('basic-token');

              return [
                  'admin'=>$adminToken->plainTextToken,
                  'update'=>$updateToken->plainTextToken,
              'basic'=>$basicToken->plainTextToken
              ];
          }
  ```
