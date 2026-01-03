```javascript
$('#data tbody').on('click', 'a', function(){
  
        let rowSelected = $(this).parents('tr');
        var rowData = tabel_.row(rowSelected).data(); });
```
