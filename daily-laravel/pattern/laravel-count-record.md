# laravel count Record by Filter

ini adalah logic function untuk membantuh menghitung berapa jumlah data yang ada di data table berdasarkan jumlah record:

###### Buat File Service dengan CountRecordService.php

```php
<?php

namespace App\Services\V1;

use Illuminate\Database\Eloquent\Model;

class RecordCountService {

    protected $model;

    public function __construct(Model $model) {
        $this->model = $model;
    }

    public function countFilterRecord ($filters)
    {
        $query =$this->model->query();

        foreach($filters as $field => $value)
        {
            $query->where($field,$value);
        }

        return $query->count();
    }
}
```

###### Buat implementasinya Di Controller.

```php
class YourNameClass extends Controller 
{
	public function countRecord(Request $request)
    {
        $filter=[
            'status' =>$request->status
        ];
        $model =new Order();
        $countRecord =new RecordCountService($model);
        $filterCountRecord = $countRecord->countFilterRecord($filter);
        return response()->json(["data"=>$filterCountRecord]);
    }
}
```
