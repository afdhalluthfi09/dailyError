# PHPMAKER
### Tombol Option
kasusnya jika kita ingin menambahkan tombol option selain dari bawaan phpmaker dimana phpmaker menyediakan tombol option standa yaitu edit,hapus,view bisa melakuakn tambahan seperti berikut:
- buka serverEvent,Client Server
- lalu cari function ListOptions dan ketikan:
```PHP
    // ListOptions Load event
function ListOptions_Load() {
	// Example:
	//$opt = &$this->ListOptions->Add("new");
	//$opt->Header = "xxx";
	//$opt->OnLeft = TRUE; // Link on left
	//$opt->MoveTo(0); // Move to first column
	$opt = &$this->ListOptions->Add("Jadwal");
	$opt->Header = "<b>Jadwal</b>";
	$opt->OnLeft = TRUE;
	
}
```
- setelah melakuakn itu kita panggil di fanction ListOption_renederd
```PHP
    function ListOptions_Rendered() {
	// Example:
	//$this->ListOptions["new"]->Body = "xxx";
	$this->ListOptions->Items["Jadwal"]->Body = "<a href='./ordersedit.php?id_orders=".CurrentTable()->id_orders->CurrentValue."'>Jadwal</a>";
}
```
