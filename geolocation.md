# Cara Buat Geolocation PHP
langsung saja:
- sediakan jquery cdn
- sediakan boostrap cdn
- lalu buat button seperti dibawah ini:
```html
<button id="btnInit" class="btn btn-block btn-info btn-lg">Koordinat Lokasi</button>
```
- buat inputan from yang bisa mengsi inputan seperti dibawah ini
```html
    <form>
        <label>Langtitude</label>
        <input id="x_x" type='text'/>
        <label>atitude</label>
        <input id="x_x" type='text'>
        <button type='submit'>kirim</button>
    </form>
```
- lalu buat script seperti dibawah ini :
```javascript
<script>  
        jQuery(window).ready(function(){  
            jQuery("#btnInit").click(initiate_geolocation);  
        });  
        function initiate_geolocation() {  
            navigator.geolocation.getCurrentPosition(handle_geolocation_query);  
        }  
        function handle_geolocation_query(position){  
			document.getElementById("x_x").value=position.coords.latitude;
			document.getElementById("x_y").value=position.coords.longitude;
					
        }  
</script>
```
### Cara menampilkan Peta 
- harus registrasi ke google developer untuk mendaptak akses token google map
- kedua ambil cdn seperti dibawah ini :
```
    <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyDefTbQnamCIdly1uRvtxmxeo2K-__H2TE&callback=initMap"
		async defer></script>
```
setelah itu lakukan inisialisasi  seperti langkah berikut :
- buat 'div' yang isinya untuk menampilkan peta di web 
```html
<body onload='petaku()'>
    <div id="petaku" style="width:100%; height:500px"></div>
```
- buat  script js yang isinya untuk menampilkan peta
```javascript
var peta;
var pertama = 0;
var jenis = "restoran";
var judulx = new Array();
var desx = new Array();
var i;
var url;
var gambar_tanda;
var markers=[];

function peta_awal(){

    var jakarta = new google.maps.LatLng(-7.8118511, 110.4091104);
    var petaoption = {
        zoom: 13,
        center: jakarta,
        mapTypeId: google.maps.MapTypeId.ROADMAP
        };
    peta = new google.maps.Map(document.getElementById("petaku"),petaoption);
}
```
- untuk bisa menandai lokasi sesuai keinginan:
```javascript
    function kasihtanda(lokasi){
    tanda = new google.maps.Marker({
            position: lokasi,
            map: peta
            
    });
    $("#x_langitude").val(lokasi.lat());
    $("#x_longtidue").val(lokasi.lng());

}
```
- untuk bisa menandai lokasi saat itu juga bisa mengunakan script dibawah ini

> - pastikan sudah membuat tombol button seperti diabwah ini 
```html
<button id='inIbutton'>Titik Lokasi</button>
```
seperti yang sudah di jelaskan di awal dari pembahasan ini
- lalu buat script yang tugasnya untuk langsung membuat marker lokasi sesuai titik 
```javascript
    function ambildatabase(){
           

                var point = new google.maps.LatLng(
                    parseFloat(document.getElementById("x_langitude").value),
                    parseFloat(document.getElementById("x_longtidue").value));
                tanda = new google.maps.Marker({
                    position: point,
                    map: peta
                });
                
}
```
untuk full scriptnya seperti dibawah ini
```javascript
<script src="https://maps.googleapis.com/maps/api/js?key=TOKEN_ANDA&callback=initMap"
		async defer></script>
<script type="text/javascript">
//google maps GIS 1.1.b by hari tri untoro
//dibuat tanggal 02 Nop 2012
var peta;
var pertama = 0;
var jenis = "restoran";
var judulx = new Array();
var desx = new Array();
var i;
var url;
var gambar_tanda;
var markers=[];

function peta_awal(){
    var jakarta = new google.maps.LatLng(-7.8118511, 110.4091104);
    var petaoption = {
        zoom: 13,
        center: jakarta,
        mapTypeId: google.maps.MapTypeId.ROADMAP
        };
    peta = new google.maps.Map(document.getElementById("petaku"),petaoption);
    
	var lokasi =new google.maps.LatLng(document.getElementById('x_langitude').value,document.getElementById('x_longtidue').value);
	var marker = new google.maps.Marker({
		position:lokasi,
		title:'hore',
	});


	
	
	google.maps.event.addListener(peta,'click',function(event){
        kasihtanda(event.latLng);
    });
	
	marker.setMap(peta);
    //ambildatabase();
}



function kasihtanda(lokasi){
    tanda = new google.maps.Marker({
            position: lokasi,
            map: peta
            
    });
    $("#x_langitude").val(lokasi.lat());
    $("#x_longtidue").val(lokasi.lng());

}

function setMapOnAll(ma){
	for(let i=0; i < markers.length; i++){
		markers[i].setMap(map);
	}
}

function clean(){
	setMapOnAll(null);
}

function hapus(){
	clean();
	markers=[];
}

function ambildatabase(){
           

                var point = new google.maps.LatLng(
                    parseFloat(document.getElementById("x_langitude").value),
                    parseFloat(document.getElementById("x_longtidue").value));
                tanda = new google.maps.Marker({
                    position: point,
                    map: peta
                });
                
}
</script>
```
adapun untuk implementasi di phpmaker nanti dibahas....

