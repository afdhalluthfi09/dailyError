# route machine leaflet js

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
