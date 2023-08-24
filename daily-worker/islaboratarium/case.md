# javascript

##### tanggal

```javascript
function formatDate(dt){
        if(dt == null){
            var date = new Date();
        }else {
            var date = new Date(dt);
        }
        // var date = new Date();
        var tahun = date.getFullYear();
        var bulan = date.getMonth();
        var tanggal = date.getDate();
        var hari = date.getDay();

        switch(hari) {
            case 0: hari = "Minggu"; break;
            case 1: hari = "Senin"; break;
            case 2: hari = "Selasa"; break;
            case 3: hari = "Rabu"; break;
            case 4: hari = "Kamis"; break;
            case 5: hari = "Jum'at"; break;
            case 6: hari = "Sabtu"; break;
        }
        switch(bulan) {
            case 0: bulan = "Januari"; break;
            case 1: bulan = "Februari"; break;
            case 2: bulan = "Maret"; break;
            case 3: bulan = "April"; break;
            case 4: bulan = "Mei"; break;
            case 5: bulan = "Juni"; break;
            case 6: bulan = "Juli"; break;
            case 7: bulan = "Agustus"; break;
            case 8: bulan = "September"; break;
            case 9: bulan = "Oktober"; break;
            case 10: bulan = "November"; break;
            case 11: bulan = "Desember"; break;
        }
        var tampilTanggal = tanggal + " " + bulan + " " + tahun;
        return tampilTanggal

    }
```

```php
public function hari($tanggal){
        $hari = date ("D", strtotime($tanggal));
   
        switch($hari){
            case 'Sun':
                $hari_ini = "Minggu";
            break;
   
            case 'Mon':
                $hari_ini = "Senin";
            break;
   
            case 'Tue':
                $hari_ini = "Selasa";
            break;
   
            case 'Wed':
                $hari_ini = "Rabu";
            break;
   
            case 'Thu':
                $hari_ini = "Kamis";
            break;
   
            case 'Fri':
                $hari_ini = "Jumat";
            break;
   
            case 'Sat':
                $hari_ini = "Sabtu";
            break;
      
            default:
                $hari_ini = "Tidak di ketahui";
            break;
        }
   
        return $hari_ini;
   
    }
```

##### loading ajax with swall

```javascript
var deferred = $.Deferred();
            $.ajax({
                url: yourUrl,
                headers: { token: yourToken },
                type: 'GET',
                data : {your data},
                dataType: "json",
                beforeSend: function () {
                    Swal.fire({
                        title: "Please Wait !",
                        allowOutsideClick: !1,
                        showConfirmButton: !1,
                        onBeforeOpen() {
                            Swal.showLoading()
                        }
                    })
                },
                success: function (data) {
                    Swal.fire({
                        icon : 'success',
                        title : 'Success',
                        // text : data.message,
                        timer : 3000
                    })
                    deferred.resolve(data);
                },
		error: function (error) {
                    Swal.fire({
                        icon : 'info',
                        title : 'Oop..!',
                        text : 'Something Wrong..',
                        timer : 3000
                    })
                    deferred.reject(error);
                }
            })
  deferred.promise();
```

##### template dataTable

```javascript
table = $('#data-table').DataTable({
        "ajax": {
            "deferLoading": -1,
            "url": yourUrl,
            "headers": { 'token': yourToken },
            "data": function (data) {
                delete data.columns;
                // Append to data
                data.nameYourField = values;
            },
            "type": "GET"
        },
        "searching": true,
        "ordering": false,
        "lengthChange": false,
        "aLengthMenu": [
            [20, 50, 100],
            [20, 50, 100]
        ],
        "columns": [
            {
                "title": "No",
                "render": function (data, type, row, meta) {
                    return meta.row + meta.settings._iDisplayStart + 1;
                }
            },
            {
                "title": "name title for header",
                "data": "name-field form your fect from database",
                "className": "text-nowrap"
            },
            {
                "title": "Action",
                "render": function (data, type, row) {
                    var html = '';
		html = '<a class="fa fa-eye" href="javascript:;"> your action</a>'
                    return html;
                },
                "className": "text-center"
            },
        ],
	buttons: [
               {
                    text: '<i class="fa fa-plus"></i>',
                    action: () => {
                        $('#add').modal('show');
                    },

                    titleAttr: 'Add Kategori',
                    className: 'add_sample',
		}
        ],
    })
```

cara nambah inputa lewat js :

```javascript
$("#addRow").click(function () {
    var html = '';
    html += '<div id="inputFormRow">';
    html += '<div class="input-groups mb-3">';
    html += '<input type="text" name="value[]" class="form-control" placeholder="kategori-3" autocomplete="off" required>';
    html += '<div class="input-group-append">';
    html += '<button id="removeRow" type="button" class="btns btn-danger">Remove</button>';
    html += '</div>';
    html += '</div>';

    $('#newRow').append(html);
});

// button remove per row
$(document).on('click', '#removeRow', function () {
    $(this).closest('#inputFormRow').remove();
});
```

##### cara buat import excel

```php
//inisialisasi
$spreadsheet = new Spreadsheet();
$sheet = $spreadsheet->getActiveSheet();
  
$sheet->mergeCells('A1:A2');
$sheet->getStyle('A1:A2')->getAlignment()->setVertical('center');
$sheet->getStyle('A1:A2')->getAlignment()->setHorizontal('center');
$sheet->getColumnDimension('A')->setWidth(6);
$sheet->mergeCells('B1:B2');
$sheet->getStyle('B1:B2')->getAlignment()->setVertical('center');
$sheet->getStyle('B1:B2')->getAlignment()->setHorizontal('center');
$sheet->getColumnDimension('B')->setWidth(35);
$sheet->mergeCells('C1:C2');
$sheet->getStyle('C1:C2')->getAlignment()->setVertical('center');
$sheet->getStyle('C1:C2')->getAlignment()->setHorizontal('center');
$sheet->getColumnDimension('C')->setWidth(13);
$sheet->mergeCells('D1:D2');
$sheet->getStyle('D1:D2')->getAlignment()->setVertical('center');
$sheet->getStyle('D1:D2')->getAlignment()->setHorizontal('center');
$sheet->getColumnDimension('D')->setWidth(10);
$sheet->mergeCells('E1:F1');
$sheet->getStyle('E:F')->getAlignment()->setHorizontal('center');
$sheet->mergeCells('G1:I1');
$sheet->getStyle('G:I')->getAlignment()->setHorizontal('center');
$sheet->getColumnDimension('I')->setWidth(25);
  
$sheet->getStyle('A1:I1')->getBorders()->getAllBorders()->setBorderStyle(Border::BORDER_THIN);
$sheet->getStyle('A2:I2')->getBorders()->getAllBorders()->setBorderStyle(Border::BORDER_THIN);
  
$sheet->setCellValue('A1', 'No');
$sheet->setCellValue('B1', 'Nama Karyawan');
$sheet->setCellValue('C1', 'Tanggal');
$sheet->setCellValue('D1', 'Hari');
$sheet->setCellValue('E1', 'Absensi');
$sheet->setCellValue('E2', 'Masuk');
$sheet->setCellValue('F2', 'Keluar');
$sheet->setCellValue('G1', 'Record');
$sheet->setCellValue('G2', ' + / -');
$sheet->setCellValue('H2', 'Jam Kerja');
$sheet->setCellValue('I2', 'Shift')
```

##### menghitung jumlah hari pada bulan di php

`cal_days_in_month(CAL_GREGORIAN, $month, $year);`

```
$data = $conn->setConnection($this->db)->join('category_sample','parameter.category_sample','=','category_sample.id')
                ->leftJoin('users','parameter.add_by','=','users.id')
                ->leftJoin('category_value', 'parameter.sub_category', '=', 'category_value.id')
                ->select('parameter.*','category_sample.name as nama_cat','category_sample.id as id_category','users.nama_lengkap as nama_orang', 'satuan', 'method', 'category_value.name as nama_value', 'nilai_minimum', 'nilai_ketidak_pastian')
                ->where('parameter.active', $request->active)
                ->where('category_sample.id', $request->category)
                ->where('category_value.id', $request->sub_category)
                ->get();
```
