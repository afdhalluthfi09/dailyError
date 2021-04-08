# PHPMAKER
### Tombol Action Page
 Kasus jika ingin melakukan selection data dan ingin memindahkan dari tabel sementara ke tabel validasi maka bisa melakukan cara seperti ini <br>
 - buka di serveEvent dan Client Script
 - lalu pergi ke function pageLoadEvent() dan tuliskan:
 ```php
    // Page Load event
    function Page_Load() {
	//echo "Page Load";
	    Language()->setPhrase("CustomActionCompleted", "Pesan Pickup Berhasil, Silahkan Menunggu Konfirmasi.");
	    $this->CustomActions["pickup"] = "Pick Up"; 
}
```
- setelah itu pergi ke function page render servent untuk mengaktifkan tombol chekout agar bisa menseleksi dan ketikan sourcode ini :
```PHP
    // Page Render event
function Page_Render() {
	//echo "Page Render";
	if (Execute("SELECT * FROM orders
						WHERE status_order='Baru'
						AND id_orders='".$this->id_orders->AdvancedSearch->SearchValue."'"
						) <> '') {
		unset($this->CustomActions["pickup"]);
	}
}
```
- lalu melakukan insert ke tabel yang dinginkan yang sudah berada pada tombol action
- cari function Row_CostumAction Lalu tulis beberapa code berikut:
```PHP
    function Row_CustomAction($action, $row) {
	// Return FALSE to abort
	if ($action == "pickup") { 
		Execute("INSERT INTO pickup (latitude,longitude) VALUES('".$row["nik_toko"]."', '".$row["jadwal"]."')");
	}
	return TRUE;
	//return TRUE;
}
```
penjelasan nanti dulu..
