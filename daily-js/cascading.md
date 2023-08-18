# JAVASCRIPT

###### perbadaan arrow function dan function manual

penggunaan this dalam javascript
funtion biasa:

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


###### cara melakukan input double dengan inputan pertama

```
<input type="text" id="input1" name="input1" />
<input type="text" id="input2" name="input2" />

<script>
const input1 = document.getElementById('input1');
const input2 = document.getElementById('input2');

input1.addEventListener('input', () => {
  input2.value = input1.value;
});
</script>

```
