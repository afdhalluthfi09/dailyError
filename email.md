# Email PHPMAKER v2020

 - aktifkan email notification di menu table 
 - lalu pergi ke tab server_client
 - email sending
 - terlihat pada function email sending di bawah ini:
 ```php
 function Email_Sending($email, &$args) {
	//var_dump($email);  var_dump($args); exit();

	if(CurrentPageID() == "edit"){
		$email->Recipient=$args["rsold"]["penerima"];
		$email->Subject =$args["rsold"]["subject"];
		$email->Content .= "\nAdded by ".$args["rsold"]["keterangan"];
		}
	return TRUE;
}
```
> terdapat dua param  yaitu $email dan $args dimana $email adalah variabel instance ke class email dan $arg intance ke class database jadi ketika ingin menulis tujuan pengiriman kita cukup menuliksan $email->Recipient dan untuk mengambil data pengirim yang di tuju cukup memanggil $args["rsold"]["namakolompengirim"] begitu juga untuk subjek ,cc, bcc, dan content dari email yang mau dikirm.

terdapat juga perbedaan dalam pengambilan nilai tabel seperti dibawah ini.
    - $args["rsnew"] : untuk nilai insert
    - $args["rsdelet"]:untuk nilai update,delet