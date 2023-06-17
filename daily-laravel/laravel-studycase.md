# laravel study case

ini adalaha beberapa kumpulan study case berdasarkan malasah yang sering saya hadapi saat membangun aplikasi,sub judul adalaha perwakilan pesan error yang sedang terjadi dan bagan tiap sub judul adalah stroy case,step solve in case,

###### Array to string conversion run db seed

jika mendapatak error seperti di bawah ini:

![1686744023317](image/laravel-studycase/1686744023317.png)

biasanya terjadi pada file factory yang sedang di kompail belum memenuhi persayaratan,dan harus diperhatikan tiap retrun dari build function yang ingin kita pakai,contoh kasus code seperti dibahwa ini:

`file LaundryFactory.php`

```php
public function definition()
    {
        $table = new Costumers();
        return [
            'costumer_id' => Costumers::factory(),
            'weight_first' => 1.00,
            'weight_second' => 300,
            'photo' =>$this->faker->imageUrl(500, 600, 'dirty clothes'),
            'status' => $this->faker->randomElement(['p','f','pu','tw',]),
            'price_first' => 8000,
            'price_second' =>80.000,
            'trigger'=>$this->faker->boolean(),
            'location_deafult' => [ $this->faker->latitude(),$this->faker->longitude() ],
            'location_now' => json_encode([$this->faker->latitude(),$this->faker->longitude()])
        ];
    }
```

pada code di atas terjadi kesalahan ekpsetasi dari filed table yang dimana hasil yang harus di kembalikan berupa json,pada line 13, tapi sebelum mengetahui apakah itu terjadi diline 13 maka kita harus mengetahui dulu expect type data yang di terima oleh table tersebut berikut step solve-nya.

* jika anda menggunakan build migrate, pastikan chek type data tiap field yang di tampung.
* pastikan type data yang di kembalikan saat menggunakan builder function faker.

dengan begitu permasalahan yang bisa diekatuhi solvnya.
