# Installasi Data Datable Laravel

berikut cara langkah-langkah untuk installasi sampai cara membuat templating basic datatable di laravel

* install package `"yajra/laravel-datatables-oracle"`
  jika saat dalam penginstallan tidak bisa berfungsi package datatable bisa diperiksa apakah packge udah terpublis atau belum,kalo belum bisa melukan langkah sepertini :

  * daftarkan provider packge ke `config/app.php` lalu cari `"provider" => [ ..` jika sudah daftarkan name classs tersebut:

    `Yajra\DataTables\DataTablesServiceProvider::class,`
  * lalu masih di file `config/app.php` cari `"aliases" =>[..]` lalu daftarkan name class package tersebut :
    `'DataTables' => Yajra\DataTables\Facades\DataTables::class,`
* pasing data melelalui restApi ke dalam datatable,carannya pergi controller yang sudah dibuat lalau tulis seperti ini:

  ```php
  <?php

  namespace App\Http\Controllers;

  use Illuminate\Http\Request;
  use Illuminate\Support\Facades\Http;
  use Yajra\DataTables\DataTables;

  class DataController extends Controller
  {
      public function getDataEmployee()
      {
          $response = Http::get('http://elaundry-api.test/api/v1/employee');
          $data = $response->json();
          $datas =$data['data'];

          return DataTables::of($datas)
                  ->make(true);
      }
  }
  ```
* setelah itu pergi ke route web.php lalu buat routing untuk bisa akses data yang udah kita pasing ke datatable:
  `Route::get('data-employee', [DataController::class,'getDataEmployee'])->name('datatable.employee');`
* lalu untuk template bladenya kita buat html basic yang dipunyai oleh datatable :

  ```html
  <table id="example2" class="table table-responsive table-bordered table-hover">
              <thead>
                  <tr>
                      <th>name</th>
                      <th>age</th>
                      <th>position</th>
                      <th>address</th>
                  </tr>
              </thead>
          </table>
  ```
* lalu tambahkan script js untuk menghubungkan data yang udah kita persiapkan di controller:

  ```javascript
  <script>
          $(function() {
              $('#example2').DataTable({
                  processing: true,
                  responsive: true,
                  serverSide: true,
                  pageLength: 50,
                  ajax: {
                      url:'{{ route('datatable.employee') }}',
                      type:"GET",
                      dataType:"json"
                  },
                  columns: [
                      {data: 'name', name: 'name'},
                      {data: 'age', name: 'age'},
                      {data: 'position', name: 'position'},
                      {data: 'address', name: 'address'},

                  ],
                  columnDefs: [
                      { title: 'name', targets: 0}
                  ]
              });
          });
      </script>
  ```
* sebelumnya  pada layout basic anda, harus ada file css dan jss library dari datatable seperti dibawah ini:

  ```html
  {{-- data table --}} <!--letakan di bagian tag head--> 
    <link rel="stylesheet" href="{{ asset('plugins/datatables-bs4/css/dataTables.bootstrap4.min.css') }}">
    <link rel="stylesheet" href="{{ asset('plugins/datatables-responsive/css/responsive.bootstrap4.min.css') }}">
    <link rel="stylesheet" href="{{ asset('plugins/datatables-buttons/css/buttons.bootstrap4.min.css') }}">

  {{-- databale --}} <!-- letakan di paling bawah body -->
      <script src="{{ asset('plugins/datatables/jquery.dataTables.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-bs4/js/dataTables.bootstrap4.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-responsive/js/dataTables.responsive.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-responsive/js/responsive.bootstrap4.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-buttons/js/dataTables.buttons.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-buttons/js/buttons.bootstrap4.min.js') }}"></script>
      <script src="{{ asset('plugins/jszip/jszip.min.js') }}"></script>
      <script src="{{ asset('plugins/pdfmake/pdfmake.min.js') }}"></script>
      <script src="{{ asset('plugins/pdfmake/vfs_fonts.js') }}"></script>
      <script src="{{ asset('plugins/datatables-buttons/js/buttons.html5.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-buttons/js/buttons.print.min.js') }}"></script>
      <script src="{{ asset('plugins/datatables-buttons/js/buttons.colVis.min.js') }}"></script>
  ```
  pastikan anda punya file fisik dari package yang dibutuhkan oleh package datatbale jika belum punya bisa menggunakan cdn dari situs resminya.
