# Trait

Trait adalah sebuah konsep dalam Pemrograman Berorientasi Objek (OOP) yang digunakan untuk mengelompokkan perilaku atau metode tertentu sehingga dapat digunakan kembali di berbagai kelas tanpa perlu mewarisi dari satu kelas utama. Dengan kata lain, trait adalah kumpulan metode yang dapat di-"impor" ke dalam kelas lain, sehingga kelas tersebut dapat memiliki perilaku tambahan tanpa harus mewarisi dari kelas lain.

Beberapa hal penting tentang trait:

1. Trait bukanlah kelas: Trait tidak dapat diinstansiasi secara langsung, dan tidak memiliki properti sendiri. Ia hanya berisi metode atau perilaku yang dapat digunakan oleh kelas lain.
2. Digunakan untuk komposisi: Trait digunakan untuk komposisi kode (code composition) daripada warisan (inheritance). Dengan menggunakan trait, Anda dapat menggabungkan metode dari beberapa trait ke dalam satu kelas.
3. Multiple Inheritance: Dalam beberapa bahasa pemrograman, seperti PHP, trait memungkinkan kelas untuk memiliki multiple inheritance, yaitu dapat menggunakan perilaku dari beberapa trait sekaligus.

Contoh penggunaan trait dalam PHP:

```php
trait Loggable {
    public function log($message) {
        echo "Logging: " . $message . "\n";
    }
}

class MyClass {
    use Loggable;

    public function doSomething() {
        // Memanggil metode dari trait
        $this->log("Doing something...");
    }
}

$obj = new MyClass();
$obj->doSomething(); // Output: Logging: Doing something...
```

Dalam contoh di atas, kita mendefinisikan sebuah trait bernama `Loggable` yang memiliki metode `log()`. Lalu, kita menggunakan trait tersebut di dalam kelas `MyClass`. Akibatnya, kelas `MyClass` dapat menggunakan metode `log()` dari trait `Loggable` tanpa perlu mewarisi dari kelas lain.

###### cara penggunaan trait pada pattern repository

Tentu! Di sini saya akan memberikan contoh penggunaan trait di dalam class child repository yang menggabungkan tiga model yang berbeda dalam sebuah transaksi. Anggap saja kita memiliki tiga model: `Order`, `Product`, dan `Payment`. Kita ingin membuat transaksi yang melibatkan semua tiga model ini.

Pertama, mari buat trait untuk transaksi:

```php
namespace App\Traits;

use Illuminate\Support\Facades\DB;

trait TransactionTrait
{
    public function performTransaction($callback)
    {
        return DB::transaction($callback);
    }
}
```

Kemudian, di dalam class child repository Anda, Anda bisa menggunakan trait ini untuk menjalankan transaksi yang melibatkan tiga model yang berbeda:

```php
use App\Traits\TransactionTrait;
use App\Models\Order;
use App\Models\Product;
use App\Repository\BaseRepository;

class OrderRepository extends BaseRepository
{
    use TransactionTrait;

    public function createOrderWithProductsAndPayment($orderData, $productData, $paymentData)
    {
        return $this->performTransaction(function () use ($orderData, $productData, $paymentData) {
            $order = $this->model->create($orderData);
          
            $products = Product::createMany($productData);
          
            // Misalkan kita memiliki relasi order dengan payment di dalam model Order
            $payment = $order->payment()->create($paymentData);

            return $order;
        });
    }
}
```

Pada contoh di atas, `OrderRepository` menggunakan trait `TransactionTrait`. Method `performTransaction` dalam trait digunakan untuk menjalankan transaksi yang melibatkan operasi pada beberapa model yang berbeda.

Pastikan Anda telah menyesuaikan kode dengan struktur dan hubungan model yang sesuai dalam aplikasi Anda. Ingat bahwa menggunakan transaksi memastikan bahwa operasi-operasi yang dilakukan dalam blok transaksi akan di-rollback jika terjadi error, sehingga basis data tetap konsisten.
pertanyaanya apakah perlu kita melakukan commit rollback dengan tryCath?Kode yang Anda tulis di dalam trait `TransactionTrait` sudah cukup untuk melaksanakan transaksi dengan bantuan `DB::transaction()` tanpa perlu menambahkan blok try-catch di dalamnya.

Metode `DB::transaction()` sendiri sudah mengatur komit dan rollback transaksi secara otomatis. Jika tidak ada exception yang di-thrown di dalam callback transaksi, maka transaksi akan di-commit. Namun, jika terjadi exception, transaksi akan di-rollback otomatis.

Dengan menggunakan trait `TransactionTrait` yang Anda tulis, Anda cukup memanggil method `performTransaction()` dan meneruskan callback yang ingin Anda eksekusi dalam transaksi. Tanpa perlu menambahkan blok try-catch, transaksi akan dikelola oleh Laravel sendiri melalui `DB::transaction()`.

Jadi, penggunaan Anda terhadap trait ini sudah cukup untuk mengelola transaksi dengan benar, dan Anda tidak perlu menambahkan blok try-catch di dalamnya.
