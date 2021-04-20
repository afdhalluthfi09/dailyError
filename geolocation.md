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
        <input type='text'/>
        <label>atitude</label>
        <input type='text'>
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