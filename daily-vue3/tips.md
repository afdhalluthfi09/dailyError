# Tips VUE3

###### reload-data

Ya, Vue dapat menerima pembaruan data dari REST API tanpa harus me-reload seluruh halaman. Anda dapat menggunakan fitur-reactive Vue, seperti reactivity dan computed properties, untuk secara dinamis memperbarui tampilan komponen Anda ketika data dari REST API berubah.

Berikut adalah langkah-langkah umum untuk menerima pembaruan data dari REST API tanpa reload:

1. Buat komponen Vue yang akan menampilkan data dari REST API. Anda dapat menggunakan metode `created` atau `mounted` untuk membuat permintaan HTTP awal untuk mengambil data dari REST API.

```javascript
<template>
  <div>
    <h1>Data dari REST API</h1>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [] // Data dari REST API akan disimpan di sini
    };
  },
  created() {
    this.fetchData(); // Panggil metode fetchData() saat komponen dibuat
  },
  methods: {
    fetchData() {
      // Lakukan permintaan HTTP ke REST API untuk mengambil data
      // Misalnya menggunakan axios
      axios.get('/api/data').then(response => {
        this.items = response.data; // Simpan data dari REST API ke variabel items
      });
    }
  }
};
</script>
```

2. Selanjutnya, untuk menerima pembaruan data secara otomatis tanpa reload, Anda dapat menggunakan teknik polling atau WebSockets. Teknik polling melibatkan melakukan permintaan HTTP berulang ke REST API dalam interval waktu tertentu untuk memeriksa pembaruan data. Sementara WebSockets memungkinkan server mengirimkan pembaruan secara real-time ke klien tanpa perlu permintaan ulang.

Untuk teknik polling, Anda dapat menggunakan `setInterval` untuk secara berkala memanggil metode `fetchData` untuk memeriksa pembaruan data dari REST API.

```javascript
created() {
  this.fetchData(); // Panggil metode fetchData() saat komponen dibuat
  setInterval(() => {
    this.fetchData(); // Panggil metode fetchData() dalam interval waktu tertentu
  }, 5000); // Contoh interval waktu 5 detik
}
```

Untuk WebSockets, Anda perlu mengonfigurasi server REST API Anda untuk menggunakan WebSocket dan mengirimkan pembaruan data secara real-time ke klien menggunakan protokol WebSocket. Di sisi Vue, Anda dapat menggunakan library seperti `socket.io` untuk menghubungkan klien dengan server WebSocket dan menerima pembaruan data secara real-time.

Dengan menggunakan teknik polling atau WebSockets, Anda dapat memperbarui data pada komponen Vue secara otomatis ketika ada perubahan di REST API tanpa harus me-reload seluruh halaman.

###### tombol redirect-whatsaap

Untuk mengarahkan pengguna ke WhatsApp dengan membawa pesan, Anda dapat menggunakan metode `window.open` untuk membuka URL WhatsApp dalam tab atau jendela baru. Berikut adalah contoh penggunaan dalam Vue 3:

```html
<template>
  <div>
    <button @click="redirectToWhatsApp">Hubungi Admin</button>
  </div>
</template>

<script>
export default {
  methods: {
    redirectToWhatsApp() {
      const phoneNumber = '1234567890'; // Ganti dengan nomor telepon admin
      const message = 'Halo, saya butuh bantuan.'; // Ganti dengan pesan yang ingin Anda kirim

      const encodedMessage = encodeURIComponent(message);
      const url = `https://api.whatsapp.com/send?phone=${phoneNumber}&text=${encodedMessage}`;

      window.open(url, '_blank');
    },
  },
};
</script>
```

Dalam contoh di atas, kita menggunakan metode `redirectToWhatsApp` yang dipanggil saat tombol "Hubungi Admin" diklik. Di dalam metode tersebut, kita membangun URL WhatsApp dengan nomor telepon admin dan pesan yang sudah terenkripsi menggunakan `encodeURIComponent`.

Kemudian, kita menggunakan `window.open` untuk membuka URL WhatsApp dalam tab atau jendela baru dengan parameter `_blank`. Hal ini akan mengarahkan pengguna ke aplikasi WhatsApp dengan pesan template yang sudah terisi.

Pastikan untuk mengganti nomor telepon dan pesan yang sesuai dengan kebutuhan Anda.

###### validate input password life

Untuk mengurangi kode yang berulang dalam metode `validateInput()`, Anda dapat membuat fungsi reusable yang menerima parameter pola regex dan pesan kesalahan. Fungsi ini dapat digunakan untuk memvalidasi berbagai input dengan persyaratan yang berbeda.

Berikut adalah contoh implementasinya:

```javascript
methods: {
  validateInput(input, regex, errorMessage) {
    if (!regex.test(input)) {
      Swal.fire('Error', errorMessage, 'error');
      return false;
    }
    return true;
  },
  validateForm() {
    const lowercaseRegex = /[a-z]/;
    const uppercaseRegex = /[A-Z]/;
    const characterRegex = /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]+/;
    const numberRegex = /[0-9]/;

    if (!this.validateInput(this.userInput, lowercaseRegex, 'Input harus mengandung setidaknya satu karakter huruf kecil.')) {
      return;
    }

    if (!this.validateInput(this.userInput, uppercaseRegex, 'Input harus mengandung setidaknya satu karakter huruf besar.')) {
      return;
    }

    if (!this.validateInput(this.userInput, characterRegex, 'Input harus mengandung setidaknya satu karakter khusus.')) {
      return;
    }

    if (!this.validateInput(this.userInput, numberRegex, 'Input harus mengandung setidaknya satu karakter angka.')) {
      return;
    }

    if (this.userInput.length < 8) {
      Swal.fire('Error', 'Input harus terdiri dari setidaknya 8 karakter.', 'error');
      return;
    }

    // Input memenuhi semua persyaratan
    // Lakukan tindakan berikutnya atau simpan input
    console.log('Input valid:', this.userInput);
  },
},
```

Dalam contoh di atas, fungsi `validateInput()` menerima tiga parameter: `input` (input yang akan divalidasi), `regex` (pola ekspresi reguler), dan `errorMessage` (pesan kesalahan yang akan ditampilkan jika validasi gagal). Fungsi ini akan mengembalikan `false` jika validasi gagal dan menampilkan pesan kesalahan menggunakan SweetAlert. Jika validasi berhasil, fungsi ini akan mengembalikan `true`.

Di dalam metode `validateForm()`, Anda dapat menggunakan fungsi `validateInput()` untuk memvalidasi input dengan berbagai persyaratan. Jika validasi gagal, fungsi akan berhenti dan menampilkan pesan kesalahan. Jika semua validasi berhasil, tindakan berikutnya dapat dilakukan.

Dengan menggunakan pendekatan ini, Anda dapat mengurangi kode yang berulang dalam metode `validateInput()` dan memudahkan penggunaan validasi pada input lain dengan persyaratan yang berbeda.

###### configurasi axios terpisah dengan vuex

```javascript
import axiosbase from 'axios';

/* configurasi costume axios agar mengurangi refactory */
// step 1: buat variable configure
const axios =axiosbase.create({  
    baseURL:'https://api.e-laundry.site/api/v1',
    headers:{
        'Content-Type':'application/json',
        'Accept':'application/json',
        'Access-Control-Allow-Origin' :'*'
    }
})

// kondisi untuk yang tidak harus pake token
axios.interceptors.request.use((config)=>{
    if(config.url === '/login-kostumer'){
        delete config.headers.Authorization;
    }else if(config.url === '/register-kostumer'){
        delete config.headers.Authorization;
    }else {
        // Jika endpoint memerlukan token, set header Authorization
        config.headers.Authorization = `Bearer ${JSON.parse(localStorage.getItem('token'))}`; // Gantikan dengan token yang valid
      }

    return config;
},(error)=>{
    return Promise.reject(error)
});

export default axios;
```

###### Permission get Location in Vue 3

Untuk meminta izin akses lokasi pengguna di Vue, Anda perlu menggunakan API Geolocation yang disediakan oleh browser. Berikut adalah langkah-langkah yang dapat Anda ikuti:

1. Impor `navigator` dari objek window:

   ```javascript
   const navigator = window.navigator;
   ```
2. Periksa apakah API Geolocation tersedia di browser:

   ```javascript
   if ('geolocation' in navigator) {
     // API Geolocation tersedia
     // Lanjutkan ke langkah selanjutnya
   } else {
     // API Geolocation tidak tersedia
     console.error('Geolocation is not supported by this browser.');
   }
   ```
3. Permintaan izin akses lokasi pengguna saat tombol tertentu ditekan atau saat komponen dimuat:

   ```javascript
   // Misalnya, saat tombol ditekan
   requestLocationPermission() {
     navigator.geolocation.getCurrentPosition(
       position => {
         // Mendapatkan posisi geografis pengguna berhasil
         const latitude = position.coords.latitude;
         const longitude = position.coords.longitude;
         console.log('Latitude:', latitude);
         console.log('Longitude:', longitude);
       },
       error => {
         // Mendapatkan posisi geografis pengguna gagal
         console.error('Error getting location:', error);
       }
     );
   },
   ```

   Dalam contoh di atas, `getCurrentPosition()` digunakan untuk meminta izin akses lokasi pengguna. Ketika izin diberikan, fungsi `getCurrentPosition()` akan memberikan respons berupa posisi geografis pengguna yang dapat Anda akses melalui objek `position.coords.latitude` dan `position.coords.longitude`.
4. Tambahkan tombol atau tindakan lain dalam komponen Vue yang akan memicu permintaan izin akses lokasi:

   ```vue
   <template>
     <div>
       <button @click="requestLocationPermission">Izinkan Akses Lokasi</button>
     </div>
   </template>
   ```

   Dalam contoh di atas, saat tombol "Izinkan Akses Lokasi" ditekan, fungsi `requestLocationPermission` akan dipanggil, dan permintaan izin akses lokasi akan dimulai.

Pastikan bahwa Anda memiliki koneksi internet yang aktif saat mencoba permintaan akses lokasi, dan pastikan juga bahwa halaman dijalankan melalui protokol HTTPs atau dijalankan di localhost, karena beberapa browser modern mensyaratkan penggunaan protokol aman untuk akses lokasi.

Dengan mengikuti langkah-langkah di atas, Anda dapat meminta izin akses lokasi pengguna dan mengakses koordinat geografis mereka melalui API Geolocation di Vue.

###### LocalStorage

```javascript
//set token
localStorage.setItem('data', JSON.stringify(data));
//get item
const data = JSON.parse(localStorage.getItem('data'));
// remove token
localStorage.removeItem('data')
```

###### Promise

Tentu! Berikut adalah contoh penggunaan class Promise dalam aksi (action) Vuex untuk mengirim data:

```javascript
actions: {
  sendData({ commit }, payload) {
    return new Promise((resolve, reject) => {
      axios.post('/api/data', payload)
        .then(response => {
          commit('SET_SUCCESS_MESSAGE', response.data.message);
          resolve(response.data);
        })
        .catch(error => {
          commit('SET_ERROR_MESSAGE', error.response.data.message);
          reject(error);
        });
    });
  }
}
```

Dalam contoh di atas, aksi `sendData` mengambil `payload` sebagai argumen, yang merupakan data yang akan dikirim ke server melalui permintaan POST.

- Aksi ini membuat sebuah instance Promise baru.
- Permintaan POST dilakukan menggunakan Axios dalam blok `then()` dan `catch()`.
- Jika permintaan berhasil (status 2xx), aksi memanggil mutasi `'SET_SUCCESS_MESSAGE'` dan melemparkan data respons ke blok `resolve()`.
- Jika terjadi kesalahan (status selain 2xx), aksi memanggil mutasi `'SET_ERROR_MESSAGE'` dan melemparkan objek error ke blok `reject()`.

Dengan menggunakan Promise, aksi tersebut mengembalikan Promise, yang memungkinkan kita untuk menggunakan `.then()` dan `.catch()` saat memanggil aksi tersebut dalam komponen Vue. Contoh penggunaan aksi ini dalam komponen Vue:

```vue
<template>
  <button @click="sendData">Send Data</button>
</template>

<script>
import { mapActions } from 'vuex';

export default {
  methods: {
    ...mapActions(['sendData']),
    sendData() {
      const data = { /* data to be sent */ };
      this.sendData(data)
        .then(response => {
          console.log('Data sent successfully:', response);
        })
        .catch(error => {
          console.error('Error sending data:', error);
        });
    }
  }
}
</script>
```

Dalam contoh di atas, kita menggunakan `mapActions` untuk memetakan aksi `'sendData'` dari Vuex ke dalam metode komponen Vue. Ketika tombol diklik, metode `sendData` dijalankan, mengirimkan data dan menangani respons atau kesalahan dengan menggunakan `.then()` dan `.catch()`.

Dengan menggunakan class Promise, Anda dapat mengelola asinkronusitas dalam aksi Vuex dan mengontrol alur eksekusi dengan menggunakan `.then()` dan `.catch()`. Ini memungkinkan Anda untuk memberikan tindakan yang sesuai setelah permintaan berhasil atau gagal.

###### Button Back Handphone untuk Vue3


Untuk mengubah tombol "Back" pada perangkat seluler menjadi tombol "Keluar" untuk web app PWA (Progressive Web App) di Vue 3, Anda dapat menggunakan API `history` pada JavaScript.

Berikut adalah contoh cara mengubah tombol "Back" menjadi tombol "Keluar" pada web app PWA Vue 3:

1. Buka file `main.js` pada proyek Vue 3 Anda.
2. Tambahkan event listener untuk menangani peristiwa `popstate` pada objek `window.history`. Event `popstate` terjadi ketika pengguna menekan tombol "Back".

```javascript
window.addEventListener('popstate', () => {
  const confirmationMessage = 'Apakah Anda yakin ingin keluar?';
  if (confirm(confirmationMessage)) {
    navigator.app.exitApp(); // Menutup aplikasi PWA pada perangkat seluler
  } else {
    history.pushState(null, null, window.location.href); // Mengembalikan ke halaman sebelumnya
  }
});
```

3. Simpan perubahan pada file `main.js` dan jalankan aplikasi PWA Vue 3 Anda.

Dengan menambahkan event listener untuk peristiwa `popstate`, ketika pengguna menekan tombol "Back" pada perangkat seluler, dialog konfirmasi akan muncul dengan pesan "Apakah Anda yakin ingin keluar?". Jika pengguna menekan "OK", aplikasi PWA akan ditutup. Jika pengguna memilih "Batal", aplikasi akan kembali ke halaman sebelumnya.

Harap dicatat bahwa metode `navigator.app.exitApp()` hanya berfungsi pada beberapa platform dan browser tertentu. Jadi, hasilnya mungkin berbeda pada setiap perangkat.
