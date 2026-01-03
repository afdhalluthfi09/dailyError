# Laravel Filter Api

ini documentasi cara membut rancangan filter yang compotuible dan flexsible untuk beberapa penarapan data yang complex,adapaun step pembuatan seperti dibawah ini:

* buat rancangan url yang ingin di adaptasikan seperti yang akan saya  buat seperti dibawah ini:

  `https://example-api.test/nameParam?nameField[variable]=value`
* setelah buat rancangan design filter maka kita buat logic filter di controller seperti dibawah ini :

  ```php
  public function index(Request $requerst)
      {
          $filter = new LaundryFilter();
          $queryItems =$filter->transform(); /* [['columnn','operator','value']] */

          if (count($queryItems) == 0) {
              return new Laundry(Laundrys::paginate());
          }else {
              return new Laundry(Laundrys::where($queryItems)->paginate());
          }

      }
  ```
* setelah itu kita buat service untuk menangani logic filternya

  * buat folder baru dalam folder `app` dengan nama `Services`
  * lalu buat folder lagi dalam `Services` dengan nama `V1` asumsinya untuk antisipasi ada skala update dimasa mendatang.
* 3
* 4
* 5
* 6
* 7
* 8
