# Laravel Factory Command

ini adalah kumpulan catatan factory command yang akan saya sering pakai saat melakuakn deploy rest-api:

* command  element
  * `$this->faker->randomElement([]) ex:$this->faker->randomElement(["I","B"])`
    syintax di atas akan melakuak random element berdasarkan range yang udah ditulis.
  * `$this->faker->email() or $this->faker->unique()->safeEmail()`
    syintax diatas akan menggenerate email random
  * `$this->faker->streetAddress()`
    syintax diatas akan menggenerate alamat random
  * `$this->faker->city()`
    syintax diatas akan menggenerate kota random
  * `$this->faker->state()`
    syintax diatas akan menggenerate desa random
  * `$this->faker->postCode()`
    syintax diatas akan menggernarate kode pos random
  * `$this->faker->numberBetween(100,10000)` return data integer
    syintax diatas akan menggenerate angka dengan range sesuai parameter yang di kasih
  * `$this->faker->imageUrl(500, 600, 'dirty clothes');`
    syintax diatas akan menggenerate url image dummy
  * `$this->faker->latitude() & $this->faker->longitude()`
    syintax diataas akan menggenerate tititk coordinate

###### logic factory untuk menentukan pilihan otomatis

```php
public function definition(){
	$random = $this->faker->randomElement(["A","K"]); //A =admin ,K kurir
	$posititon =$random == "A" ? "admin" : "kurir";
        return [
            "name" =>$this->faker->name(),
            "age" =>
            "postition" =>$postition,
        ];
    }
```

###### logic factory untuk membuat relasi table

NameClassModels::factory() dengan menuliskan syntaks tersebut maka saat melakuak generate data dump akan terisi otomati data tersebut berikut step untuk melaukanya:

* buat relasi pada model
  pada kasus ini adalah table laundri memeiliki foreignkey dengan table costumer maka kita bisa menulis syntax pada kedua model table tersebut seperti pada gambar dibawah ini:

  * model laundry:
    kita buat relasi dengan type belongsto karena laundry boleh memiliki banyak data costumers

    ```php
    class Laundrys extends Model
    {
        use HasFactory;

        protected $fillable=[];

        public function costumers ()
        {
            return $this->belongsTo(Costumers::class,'costumer_id','id');
        }
    }
    ```
  * model costumers :

    kita harus  buat relasi dengan type hasMany karena costumer hanya bisa memiliki satu data darilaundry.

    ```php
    class Costumers extends Model
    {
        use HasFactory;

        protected $fillable =[];

        public function laundrys ()
        {
            return $this->hasMany(Laundrys::class);
        }
    }
    ```
* buat data dummy di factory laundry seperti berikut:

  ```php
  public function definition()
      {
          return [
              'costumer_id' => Costumers::factory(),
              'weight_first' => 1.00,
              'weight_second' => 300,
  	    ....
          ];
      }
  ```

  dengan begitu maka foreign key di data table laundry akan mengikuti data dari costumers tanpa mengsi secara manual.
* daftarkan class factory tersebut ke seeder yang saat ini saya buat terpisah seperti berikut:

  * buat file seeder specific dengan comman:

    `php artisan make:seeder LaundryFactory`
  * setelah  itu daftarkan factorti tersebut ke dalam seedr yang udah di buat tadi:

    ```php
    public function run()
        {
            //
            Laundrys::factory(10)->create();
        }
    ```
  * lalu pergi ke main dari file seeder dan daftar kan seeder yang udah dibuat tadi:

    ```php
    public function run()
        {
            $this->call(LaundrySeeder::class);
        }
    ```

    setelah iu jalan kan command berikut `php artisan db:seeder` dengan begitu kedua table yang berelasi sudah terisi otomastis, pastikan model costumer sebelumnya juga udah di buat factorinya.

###### logic factory mengambil data field specific di table relasi.

kasusnya: jika kita ingin mengisi data pada field table secara specific yang dimana data yang disi adalah data yang dimiliki oleh table berelasi dengan table tersebut ex:

table laundry memeliki filed yang diberinama location_default, dimana location default ini  datanya adalah coordinate yang di isi oleh costumer saat melakukan register,problemnya bagaiamana factory membuat dummy data  yang  serupa dengan data yang ada pada table costumer. berikut cara penyelesaiannya:

* pada kedua model tersebut dibuatkan relasi sperti berikut:

  ```php
  //pada model costummers
  class Costumers extends Model
  {
      use HasFactory;

      protected $fillable =[];

      public function laundrys ()
      {
          return $this->hasMany(Laundrys::class);
      }
  }

  //pada table laundry

  class Laundrys extends Model
  {
      use HasFactory;

      protected $fillable=[];

      public function costumers ()
      {
          return $this->belongsTo(Costumers::class,'costumer_id','id');
      }
  }
  ```
* setalah itu kita bisa membuat data dummy di factory laundry seperti dibawah ini:

  ```php
  public function definition()
      {
          return [
              ...
              'location_deafult' => function (){ return Costumers::all()->random()->location;},
              'location_now' => json_encode([$this->faker->latitude(),$this->faker->longitude()])
          ];
      }
  ```
* setalah itu daftarkan ke seeder main,lalu jalan kan command `db:seeder`
