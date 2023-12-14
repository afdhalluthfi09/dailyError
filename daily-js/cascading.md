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

disbale click

```


document.onkeydown = function(e) {
     if (e.ctrlKey && 
         (e.keyCode === 67 || 
             e.keyCode === 86 || 
             e.keyCode === 85 || 
             e.keyCode === 117)) {
         return false;
     } else {
         return true;
     }


};
$(document).keypress("u",function(e) {
  if(e.ctrlKey)
  { return false;}
  else{
    return true;
  }
});

 if (!$('body').hasClass('debug_mode')) {
        var _z = console;
        Object.defineProperty(window, "console", {
            get: function () {
                if ((window && window._z && window._z._commandLineAPI) || {}) {
                    throw "Nice trick! but not permitted!";
                }
                return _z;
            },
            set: function (val) {
                _z = val;
            }
        });
    }

    document.addEventListener('contextmenu', event => event.preventDefault());
```
###### currency mata uang menjadi format IDR Rp.

```
function formatDollar(number){
    return new Intl.NumberFormat('id-ID',{style:'currency',currency:'IDR'}).format(number);
}
```
