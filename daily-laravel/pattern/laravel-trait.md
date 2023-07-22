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
