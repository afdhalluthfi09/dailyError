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

###### menangkap error dengan log.

Untuk mencatat log kesalahan yang terkait dengan masalah CORS di Laravel, Anda dapat menggunakan fungsi `Log::error()` untuk mencatat pesan kesalahan ke file log aplikasi Laravel. Anda dapat menambahkan baris kode berikut di dalam blok `catch` pada bagian kode yang menangani permintaan Anda:

```php
catch (\Throwable $th) {
    DB::rollBack();
    Log::error('Error saat memproses permintaan: ' . $th->getMessage());
    return response()->json(["message" => $th->getMessage()]);
}
```

Dengan cara ini, ketika terjadi kesalahan dalam memproses permintaan, pesan kesalahan akan dicatat ke file log aplikasi Laravel, dan Anda dapat melihatnya di file log yang sesuai. Biasanya, log kesalahan akan disimpan di file `storage/logs/laravel.log` atau sesuai dengan konfigurasi log Anda.

Dengan mencatat log kesalahan seperti ini, Anda dapat memeriksa log aplikasi Laravel untuk melacak apakah ada masalah CORS yang menyebabkan kesalahan di sisi server saat permintaan dilakukan dari admin.e-laundry.site. Jika ada masalah CORS, Anda akan melihat pesan kesalahan terkait dalam log, dan dari situ Anda dapat mencari solusi untuk mengatasi masalah tersebut.

###### menggunakan index looping dari foreach untuk pengurutan nomor.

```php
@forelse ($costumer as $item)
                <tr>
                    <td>{{$loop->index +1}}</td>
                    <td>{{$item['name']}}</td>
                    <td>{{$item['email']}}</td>
                    <td>{{$item['address']}}</td>
                    <td>{{$item['address']}}</td>
                    <td>{{$item['age']}}</td>
                    <td>{{$item['location']}}</td>
                    <td><button class="btn btn-edit btn-success p-2">Edit</button></td>
                </tr>
            @empty
                <tr>belum adad</tr>
            @endforelse
```

pada tempalte foreac pada blade ada fitur yang namanya $loop dimana kita bisa manggil didalamnya index array hasil pecahan dari perulangan
