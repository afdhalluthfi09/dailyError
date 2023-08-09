# JAVASCRIPT

###### perbadaan arrow function dan function manual

* penggunaan this dalam javascript
  funtion biasas:

  ```javascript
  // funtion biasa
  <button id="button"  data-id="eeeewew" >halo</button>
  <script>
  $('#button').on('click',function(){
     $(this).data('id');
  })
  </script>
  ```
  arrow function:

  ```javascript
  //arrow function
  <button id="button"  data-id="eeeewew" >halo</button>
  <script>
  $('#button').on('click',(e)=>{
      let thisForm =$(e.currentTarget); cara inisialisasi form
     thisForm.data('id');
  })
  </script>

  ```
*
