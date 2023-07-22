# PHPMAKER
### Install Options Web
kasus ini dilakukan ketika ingin web bisa dijadikan aplikasi di mobile yang harus dibutuhkan  adalah:
- service.js
- worker.js
- manifest.js
- domain sudah ssl (https)
---
saat pengujian ini saya menggunakan phpmaker sebagai platform buat aplikasi web  service.js kita jadikan pelayanan penyimpanan dan pengaturan choockies aplikasi kita dan script pelayanan ini sering disebut serviceWorker. 

berikut caranya:

- kita melakukan pengecekan apakah suda support atau belum web kita dengan serviceworker buat file dengan nama service.js 
```JAVASCRIPT
    if ('serviceWorker' in navigator) {
    window.addEventListener('load', function() {
      navigator.serviceWorker.register('../../sw.js').then(function(registration) {
        // Registration was successful
        console.log('ServiceWorker registration successful with scope: ', registration.scope);
      }, function(err) {
        // registration failed :(
        console.log('ServiceWorker registration failed: ', err);
      });
    });
  }
```
- buat script manifest.js
```JSON
    {
	"short_name": "GONIHON",
	"name": "Gonihon",
	"icons": [
	  {
		"src": "images/icon-192.png",
		"type": "image/png",
		"sizes": "192x192"
	  },
	  
	  {
		"src": "images/icon-48.png",
		"type": "image/png",
		"sizes": "48x48"
	  },
	  {
		"src": "images/icon-72.png",
		"type": "image/png",
		"sizes": "72x72"
	  }
	],
	"start_url": "http://localhost/siswagonihon/tips.php",
	"background_color": "#3367D6",
	"display": "standalone",
	"theme_color": "#ea0b0b"
  
  }
```
- lalu buat file worker.js yang diletakan pada serverkita berada
```JAVASCRIPT
    const cacheName = 'v1';
const urlsToCache = [
    'koperasi/adminlist.php',
    'koperasi/fcss/koperasi.css',
    'koperasi/fcss/service.js'
]

self.addEventListener('install', e=>{
    e.waitUntil(
        caches
            .open(cacheName)
            .then(cache=>{
                console.log('daftar cache');
                cache.addAll(urlsToCache);
            })
            .then(() => self.skipWaiting())
    )
});

self.addEventListener('activate', e => {
    console.log('service worker : telah aktivasi')

    e.waitUntil(
        caches.keys().then(cacheNames => {
            return Promise.all(
                cacheNames.map(cache => {
                    if (cache !== cacheName) {
                        console.log('Service Worker: menghapus cache lama');
                        return caches.delete(cache);
                    }
                })
            )
        })
    )
})


self.addEventListener('fetch', e => {
   e.respondWith(fetch(e.request).catch(()=>caches.match(e.request)))
})
```
penjelasan nanti dulu..

