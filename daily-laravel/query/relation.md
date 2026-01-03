# Relation at Elequent Model Laravel

###### one to one

Dalam hubungan ini, satu record pada model berhubungan dengan satu record pada model lain. Misalnya, hubungan antara User dan Profile, di mana satu pengguna memiliki satu profil.

hubungan "One-to-One" (Satu-ke-Satu) pada Eloquent di Laravel. Dalam hubungan ini, setiap record pada model memiliki hubungan dengan satu record pada model lain, dan sebaliknya. Ini berarti bahwa setiap entitas pada model akan memiliki hubungan yang unik dengan satu entitas pada model lainnya.

Misalnya, dalam hubungan "One-to-One" antara `User` dan `Profile`, satu pengguna (`User`) akan memiliki satu dan hanya satu profil (`Profile`). Ini juga berarti bahwa satu profil tidak dapat berhubungan dengan lebih dari satu pengguna.

Jika Anda mencoba menambahkan profil baru dengan id user yang sama, Anda akan mengalami konflik pada level database. Model Eloquent pada Laravel akan mencoba untuk mengikuti aturan hubungan yang telah Anda tetapkan, dan jika Anda telah mendefinisikan hubungan "One-to-One", maka sistem akan memastikan bahwa satu user hanya memiliki satu profil, dan Anda akan melihat kesalahan atau peringatan yang sesuai jika Anda mencoba untuk melanggar aturan tersebut.

Contoh kode untuk mendefinisikan hubungan "One-to-One" antara `User` dan `Profile`:

**Model User:**

```php
class User extends Model
{
    public function profile()
    {
        return $this->hasOne(Profile::class);
    }
}
```

**Model Profile:**

```php
class Profile extends Model
{
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

Dalam contoh ini, setiap `User` memiliki satu `Profile` (hasOne), dan setiap `Profile` dimiliki oleh satu `User` (belongsTo).

###### one to many

Dalam hubungan ini, satu record pada model memiliki banyak record yang berhubungan pada model lain. Misalnya, hubungan antara User dan Post, di mana satu pengguna dapat memiliki banyak postingan.

Konsep One-to-Many dalam hubungan model pada Laravel dapat diterapkan pada kasus pembuatan komentar pada tiap postingan. Dalam hal ini, satu postingan dapat memiliki banyak komentar, sehingga ada hubungan "satu ke banyak" antara model Postingan (Post) dan Komentar (Comment).

Berikut adalah cara Anda bisa menerapkannya:

**Model User (One-to-One dengan Profil):**

```php
class User extends Model
{
    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    // ...
}
```

**Model Post (One-to-Many dengan Komentar):**

```php
class Post extends Model
{
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }

    // ...
}
```

**Model Comment (Many-to-One dengan User dan Post):**

```php
class Comment extends Model
{
    public function user()
    {
        return $this->belongsTo(User::class);
    }

    public function post()
    {
        return $this->belongsTo(Post::class);
    }

    // ...
}
```

Dalam kasus ini:

- Model User memiliki hubungan One-to-One dengan Profil.
- Model Post memiliki hubungan One-to-Many dengan Komentar.
- Model Comment memiliki hubungan Many-to-One dengan User (user yang melakukan komentar) dan Post (postingan yang dikomentari).

Dengan konfigurasi hubungan ini, Anda dapat dengan mudah mengakses komentar dari suatu postingan atau mengetahui pengguna yang melakukan komentar pada suatu komentar. Misalnya:

```php
// Mengambil semua komentar dari suatu postingan
$comments = $post->comments;

// Mengambil pengguna yang melakukan suatu komentar
$user = $comment->user;
```

Penting untuk diingat bahwa hubungan Eloquent ini memungkinkan Anda untuk dengan mudah mengambil, menyimpan, dan mengelola data terkait di antara model Anda, membuat struktur basis data Anda lebih teratur dan lebih mudah dikelola.
