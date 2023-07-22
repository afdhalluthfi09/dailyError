# Script Falidasi
---
Buat Falidasi dengan Script 
>cara ini digunakan untuk melakukan pembersihan inputan yang tidak di inginkan tinggal menambahkan attribut onkeypress='return falidasiInputan(event)' pada element inputan 
```javascript
    function falidasiInputan(evt){
        var charCode=(evt.which) ? evt.which : event.keyCode
        if (charCode > 31 && (charCode < 48 || charCode > 57))
        return false;
        return true;
}
```