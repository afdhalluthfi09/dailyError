# Daily JS

###### Route Machine JS

ini adalah base template jika ingin menggunakan routmachine dengan javascript.
maka dari itu sediakan dua hal untuk bisa menjalankannya

* cdn leaflet:
  * [https://unpkg.com/leaflet@1.7.1/dist/leaflet.js](https://unpkg.com/leaflet@1.7.1/dist/leaflet.js "leaflet.js")
  * [https://unpkg.com/leaflet@1.7.1/dist/leaflet.css](https://unpkg.com/leaflet@1.7.1/dist/leaflet.css "https://unpkg.com/leaflet@1.7.1/dist/leaflet.css")
* cdn route machine leaflet.js :
  * [https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.js](https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.js "https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.js")
  * [https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.css](https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.css "https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.css")

###### tempalte html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.css" />
    <title>document</title>
</head>
<body>
    <div id="map" style="width: 100%; height: 300px;"></div>
    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/leaflet-routing-machine/dist/leaflet-routing-machine.js"></script>
    <script>
        // Initialize the map
        var map = L.map('map').setView([51.505, -0.09], 13);

        // Add a tile layer (e.g., OpenStreetMap)
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: 'Map data © <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors',
            maxZoom: 18,
        }).addTo(map);
        L.Routing.control({
            waypoints: [
                L.latLng(51.5, -0.1), // Start point
                L.latLng(51.51, -0.12) // End point
            ],
            routeWhileDragging: true // Update route in real-time while dragging waypoints
        }).addTo(map);
    </script>
</body>
</html>
```

###### menangkap pesana errorr dengan fetch pada sisi client.

kita cukup menggunakan try catch unutk menangkap error yang terjadi pada sisi client

```javascript
async function addData(inputData){

        try {
            let headersList = {
                "Accept": "application/json",
                "Authorization":`Bearer ${csrf.data('session')}`,
                "Content-Type":"application/json",
                "withCredentials":true
            }
            const response = await fetch(`${import.meta.env.VITE_URL_LOKAL}/employee`,{
                method:'POST',
                body:JSON.stringify(inputData),
                headers:headersList
            })

            const data  = await response.json();

            if(response.ok){
                console.log(data);
                Swal.fire({
                    icon: 'success',
                    title: 'Data Updated',
                    text: data.message,
                }).then((result) => {
                    if (result.isConfirmed) {
                        instanceEmployee.ajax.reload()
                    }
                });
            }else{
                let errors =data.errors;
                let errorMessages='';
                for(let index in errors){
                    errorMessages += errors[index]+'<br>';
                }
                Swal.fire({
                    icon: 'warning',
                    title: data.message,
                    html:errorMessages,
                }).then((result) => {
                    if (result.isConfirmed) {
                        instanceEmployee.ajax.reload()
                    }
                });
            }
        } catch (error) {
            Swal.fire({
                icon: 'error',
                title: error,
            }).then((result) => {
                if (result.isConfirmed) {
                    instanceEmployee.ajax.reload()
                }
            });
        }
    }
```
