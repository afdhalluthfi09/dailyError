# PHPMAKER PDO custom

>kasus ini jika ada yang meminta koneksi database dengan PDO
---
### Buat Koneksi
bebas caranya menulis koneksinya dengan model yang diketahui anda, adapun saya disini menggunakan dengan model konsep .env mesisahkan setiap variabel yang sensitif agar tidak bisa di baaca oleh orang rempong hehe

```PHP
$host =$CONFIG['CONNECTION_INFO']['DB']["host"];
		$db_name =$CONFIG['CONNECTION_INFO']['DB']["db"];
		$db_pass =$CONFIG['CONNECTION_INFO']['DB']["pass"];
		$db_user =$CONFIG['CONNECTION_INFO']['DB']["user"];
		$db_port =$CONFIG['CONNECTION_INFO']['DB']["port"];
```
setelah  buat koneksi seperti ini:
```PHP
	$conn =mysqli_connect($host,$db_user,$db_pass,$db_name,$db_port);
		if(!$conn){
			die('gagal sambung'.mysqli_connect_error()); 
		}
```
### buat model PDO ex:contoh
dengan asusminya anda sudah punya kasus yang inigin di implementasikan, disini saya kasunay ingin membinding data agar adata yang saya kirim melalui get bisa proses dulu sebelum mengualarkan result dari query yang saya buat
berikut contohnya:
```HTML
<table class="table ew-table" style="background-color:#e7e7ff" width="100%">
		<thead style="background-color:#3e3276">
			<tr style="color:#ffff">
				<th class="ew-table-header-cell">Nama Kompentensi</th>
				<th class="ew-table-header-cell">Nilai</th>
			</tr>
		</thead>
		<?php
		$getbind =mysqli_real_escape_string($conn,Get('b'));
		$stmt=$conn->prepare("SELECT * FROM temp_trans_skor WHERE  md5(nik)= ?");
		$stmt->bind_param('s',$getbind);
		$stmt->execute();
		$result =$stmt->get_result();
		$row =$result->fetch_array(MYSQLI_ASSOC);
		
		if (empty($row['nik'])) {

			echo "<script>
  				alert('data masih kosong');
  				window.location.href='umkm_data_dirilist.php';
  		     </script>";
		}

		$no =1;
		while ($r = $result->fetch_assoc()) {

			echo "<tr class='ew-table-row'>
			<td>$r[jenis_nilai]</td>
			<td>$r[nilai]</td>
		    </tr>";
			$no++; 	
		}
		echo "
  </tbody>";
  	$result->close();
  	$stmt->close();
		?>


	</table>
```
penjelasan nanti aja..