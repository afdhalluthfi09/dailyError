## validasi duplicat dan insert by restApi melalui raw_inserting
```php
function Row_Inserting($rsold, &$rsnew)
{
    // Enter your code here
    // To cancel, set return value to false
            $if_exists  = ExecuteScalar("SELECT count(*) FROM tasks where TaskID = '".$rsnew["TaskID"]."' and TaskName = '".$rsnew["TaskName"]."' ");
        if($if_exists > 0)
        {
    		$this->CancelMessage = "Opss Data". $rsnew['produk_nama']." Yang Disi Udah Ada Mbok Di Ganti";
    		return false;
    	 }else{
            $curl = curl_init();
            curl_setopt_array($curl, array(
                CURLOPT_URL => 'https://sibakuljogja.jogjaprov.go.id/restapi/api2',
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_ENCODING => '',
                CURLOPT_MAXREDIRS => 10,
                CURLOPT_TIMEOUT => 0,
                CURLOPT_FOLLOWLOCATION => true,
                CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
                CURLOPT_CUSTOMREQUEST => 'POST',
                CURLOPT_POSTFIELDS =>'{
                    "data":"INPUTDATA",
                    "nik":"'.$rsnew["TaskID"].'",
                    "nama":"Erfin",
                    "alamat":"alamatku dimana",
                    "no_hp":"0987656788",
                    "email":"emdil@gmail.com"
                  }',
                CURLOPT_HTTPHEADER => array(
                    'Authorization: Bearer ',
                    'Content-Type: application/json'),
            ));
            $response = curl_exec($curl);
            curl_close($curl);
            $echo = json_decode($response,true);
            return $echo['Status'];

    	 }
}
```

## untuk pesan jika berhasil bisa melakukan di raw_inserted
```php
function Row_Inserted($rsold, &$rsnew)
{
    //Log("Row Inserted");
    $this->setSuccessMessage($rsnew["TaskName"]." Berhasil Ditambhakan");
}
```