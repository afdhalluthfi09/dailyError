# PHPMAKER
### cara consume Api
kasusnya ini dilakukan untuk melakukan penggunaan Api pada dropdown inputan
```JAVASCRIPT
    $.ajax({

	url: "alamat.com/enpoint/",
	method: "POST",
	timeout: 0,
	headers: {
		Authorization: "Bearer Yang Anda Miliki",
		"Client-Id": "112233"
		},
	dataType:"json",
	success:function(response){
		console.log(response.schedules)
		$('select[name="x_jadwal"]').empty();
		$.each(response.schedules,function(key,val){
		if(!val.libur){
			$('select[name="x_jadwal"]').append('<option value="'+val.clock+'">'+val.clock+' (s.d '+val.until+')</option>');
		}else{
			$('select[name="x_jadwal"]').append('<option disabled>'+val.clock+' Penejemputan libur</option>');
		}
			
		})
		
	}
})
```
penjelasanya nanti