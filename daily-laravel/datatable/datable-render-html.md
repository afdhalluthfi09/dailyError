# Render Html Di Raw Datatable Laravel

kebutuhan costumisasi pada databale udah tidak diragukan oleh karan itu kita harus tau cara menggunakannya:

* pergi ke script yang  databale yang anda buat contohnya seperti ini:

  ```php
  public function getData()
      {
          $response = Http::get('http://elaundry-api.test/api/v1/laundry'); // Ganti dengan URL REST API sesuai kebutuhan
          $data = $response->json();
          $datas =$data['data'];
          return DataTables::of($datas)
                  ->editColumn('costumer',function($datas) {
                      return $datas["costumer"]["name"];
                  })
                  ->editColumn('photo', function ($datas) {
                      return '<img src="'. $datas['photo'] .'" width=200 height=200 />';
                  })
                  ->addColumn('action',function(){
                      return '<div class="d-flex flex-row gap-4">
                                  <button class="btn btn-success gap-3">Order</button>
                                  <button class="btn btn-primary">Order</button>
                              </div>';
                  })
                  ->rawColumns(['photo','action'])
                 ->make(true);
      }
  ```
  perhatikan sintak `editColumn()` ,sintak ini berfungsi untuk melakukan perubahan value pada kolum yang udah terpasing data,  jika sudah melakuakn perubahan banyak kesalaha yang sering di lupaka yaitu mendaftarkn column yang udah di rubah dengan syintax `rawColumns()`
