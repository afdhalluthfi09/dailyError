# Visible Colum or Raw in DataTable in Laravel

kasus ini mungkin dibutuhkan saat kita lagi mengerjakan satu data table yang mempunya filter beragam sehgingga tampilan mengikuti kondis filter,berikut beberapa kasus yang saya alami:

###### visible column datatabale melalui javascript:

```javascript
$('#your-datatable-id').DataTable({
        // DataTables configuration options
        // ...
        // ...
        // Add the "drawCallback" function to modify the table after each redraw
        "drawCallback": function(settings) {
            var table = $(this);
            var data = table.DataTable().rows().data();

            // Loop through each row of the table
            table.DataTable().rows().every(function(rowIdx, tableLoop, rowLoop) {
                var rowData = this.data();
		/**
		column(columnIndex) :pada kodingan dibawah ini columindex yang dimaksud index header dari 0 sampai jumlah tertentu
		*/
              	let columnIndex ='your_name_header_specific'
                // Check the condition for the specific column in each row
                if (rowData[columnIndex] === conditionValue) {
                    // Hide the specific column by setting its visibility to false
                    table.DataTable().column(columnIndex).visible(false);
                } else {
                    // Show the specific column if the condition is not met
                    table.DataTable().column(columnIndex).visible(true);
                }
            });
        }
    });
```
