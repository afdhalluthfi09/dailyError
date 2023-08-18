# Laravel Query

ini kumpulan-kumpulan case yang saya alami selama mengerjakan projet berikut beberapa solve dari permasalahan saya:

###### cek data field kosong spesifik pada table :

```php
$isEmpty = DB::table('laundrys')
                ->whereJsonLength('location_now', '=', 0)
                ->exists(); //hasil return dari variabel ini adalah boolean
//maka kita bisa menggunakan variable ini untuk kondisi yang kita butuhkan
if($isEmpty) {
  // tulis sesuatu..
}else{
  // tulis sesuatu..
}
```

###### cara insert multiple record.

```php
public function addKurikulum ($kelas_id,$request)
    {
        $data =[];
        for ($i=0; $i < count($request->kurikulum); $i++) {
            # code...
            echo $i .$request->kurikulum[$i].'<br>';
            $data [] =[
                'kelas_id'=>$kelas_id,
                'kurikulum'=>$request->kurikulum[$i]
            ];
        }
        KurikulumModel::insert($data);
    }
public function updateKurikulum ($request)
    {
        /*
            dear develop sekolahskill after here..
            untuk kasus update multipel record yang hanya mengandalkan single column table disarankan melakukan
            rselove seperti di bawah ini,walaupun ini belum terbukti work it untuk di setiap project, tapi
            ini sudah lebih dari cukup,
            walaupun resikonya adalah bertambahnya index dari database itu sendiri tapi bisa di pastikan beberapa
            aplikasi tidak akan sering melakukan perubahan besar secara terus menerus untuk kasus multple recoerd seperti kasus
            ini..
        */

        $data=[];
        KurikulumModel::where('kelas_id',$request->kelas_id)
                    ->delete();

        for($i=0;$i <count($request->kurikulum); $i++){
            $data [] =[
                'kelas_id'=>$request->kelas_id,
                'kurikulum'=>$request->kurikulum[$i]
            ];
        }
        KurikulumModel::insert($data);
    }
```
