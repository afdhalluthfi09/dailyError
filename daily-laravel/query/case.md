# query all

jika query all menggunakan soft delete maka dan tidak ada data yang muncul maka gunakan log query untuk tracing


```
DB::enableQueryLog();
$dataModel = EventModel::all();
dd(DB::getQueryLog()); //resultnya :"query" => "select * from `event_models` where `event_models`.`deleted_at` is null"
```
