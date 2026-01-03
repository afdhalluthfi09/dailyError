# Laravel-Alert.

ada beberpa packgae yang saya sering gunakan dalam membuat notif untuk aplikasi yang sedang saya bangun:

* sweetalelert.js
  * cara instalasinya cukup menggunakan cdn:

    `<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>`
  * cara basic yang digunakan untuk sweetalert

    ```javascript
    Swal.fire({
                icon: 'success',
                title: 'Data Updated',
                text: data.message,
               })
    ```
    jika ingin menggunakan tingkat lanjut kasusnya seperti setalah muncul popup alert lalu ingin ada logic lain setelah menekan tombol oke di alert seperti dibawah ini penulisnaya:,

    ```javascript
    Swal.fire({
                icon: 'success',
                title: 'Data Updated',
                text: data.message,
         }).then((result) => {
               if (result.isConfirmed) {
    		//lakukan penulisanya disini
    	     }
     });
    ```
  * selamat pusing selanjutnya.
* notiflain
