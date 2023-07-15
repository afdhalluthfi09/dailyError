# Laravel-RestApi

ini berupa kumpulan-kumpulan problem selama  membangun aplikasi:

###### Parsing Data Tidak Sampai.

saat melakukan post,update data, kita butuh melakukan pengiriman data ke body UrlEndpoit,tapi saat tiba dalam melakukan proses itu data yang kita kirim lewat body urlEndpoint tidak sampai, itu disebabkan kita belum menambahakn di dalam header type data yang kita kirim kan, berkitut cara melakukanya:

```javascript
async function fetchData(updateData) {
                let headersList = {
                    "Content-Type": "application/json", //memasang type body pengriminan data
                    "Access-Control-Allow-Origin": "*"
                }
                let bodyContent = JSON.stringify({
                    "laundry_id": updateData.laundry_id,
                    "empolyee_id": updateData.empolyee_id,
                    "status": updateData.status,
                    "photo": updateData.photo
                });
                await fetch(`urlEndpoint`, {
                        method: "PUT",
                        headers: headersList,
                        body: bodyContent
                    })
                    .then(response => response.json())
		    .then((data)=>{data.data})
 		    .catch((error)=>{console.log(error)})
		})
}
```
