# Membuat Option Hide Jika Tidak Di Butuhkan
kasusnya jika ada pilihan yang memungkinkan "PILIHAN" ada jika di butuhkan maka cara implementasinya sebagai berikut:

- pastikan model inputanya adalah sebuah option,dropdwon,radio button
- tambahakan atribut pada tag componen option tersebut onchange='namaFunction()'
```javascript
onchange='CheckColors(this.value);'
```
- buat inputan (hide) untuk sebagai option jawaban yang jika di butuhkan seperti pada gambar dibawah ini :
```HTML
<input type="text" name="color" id="color" style='display:none;'/>
```
- pada intputan (hide) tambhakan attr style='display:none;' 
- lalu buat script seperti gambar dibawah ini :
```javascript
<script type="text/javascript">
function CheckColors(val){
 var element=document.getElementById('color');
 if(val=='Tidak ada'||val=='lainya')
   element.style.display='block';
 else  
   element.style.display='none';
}

</script> 
```
- lihat hasilnya.