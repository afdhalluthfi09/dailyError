# LARAVELMIX VUE
Daily Untuk Project ini agak sedikir rumit untuk saya sebagai pemula mari kita bedah tiap error yang udah tertangani maupun belum nantinya:

## akses/assign varibale di template VUE
kasusnya ketika kita ingin mengakse data pada variable yang sudah kita buat dalam script javascrip untuk di set value yang kita setting, value tersebut tidak bisa terisi dengan sempurna, dan sudah terpecahkan berikut bentuk script tersebut :
```javascript
    export module {
        data(){
            return {
                variable1:[],
                varibal2:''
            }
        },

        computed:{
            setData:()=>{
                this.variable1 =any value;
            }
        }
    }
```
tulisan code tersebut tidak bisa menyeting data yang mau kita isi di karenakan untuk versi vuejs v2, setiap variable kosong harus terlebih dahulu di periksa guna mencegah bentroknya nama varibale yang sama dalam satu sycle (menurut saya saat ini), dan dalam standar penulisan code saat ini berlaku untuk javascrip adalaha setaip variabel yang belum ada isinya diset dengan nilai null sebagai deafultnya. dan untuk problem solvenya sebagai berikut:
```javascript
    export module {
        data(){
            return {
                variable1:null,
                varibal2:null
            }
        },

        computed:{
            setData:()=>{
                if( this.variable1 == null){
                    this.variable1 =any value;
                }else{
                    console.log('data udah terisi value');
                }
            }
        }
    }
```
dengan melakukan pengecekan statement pada saat pengsian value berlangsung ini akan mencega terjadi bentrok assign value pada tiap variable yang telah di panggil atau dalam satu sycle.

## Cahce WebApp Yang Susah Di Hapus
pada kasus ini saya mendapati ketika selama pengembangan  berlangsung dengan framework javascripit, banyak cache yang agak susah di hapus,disebakan penyimpanan data yang di browser menganggap file yang di local kita lakukan tidak dianggap terajdi perubahan karan browser secara sederhananya akan melakuakan pengecekan apakah file dengan extensi serupa (ex: js,css) menagalami perubaha, jika tidak terjadi sesuatu maka browsser akan mempertahan data yang sudah tersimpan di localbrowser, sedangakan kita sudah melakukan banayak perubaahan dengan ekstensi yang sama agar ini tidak terjadi kita perlu menambahakn script webpack kita sepertini ini:
```javascript

    //versi 1:
    .mix.version()
    
    //versi 2:
    if(mix.inProduction()){
        mix.version();
    }
```
maka hasil dari akhir dari file nanti akan mempunay code uniq sepery ini:
ex: script.js?234AdcWqq atau styel.css?234AdcWqq dengan begini crash caching di webapp kita sudah bukan lagi masalah.

## tidak bisa fetch data api:
kasus ini sering kita akanjumpai jika tidak paham konsep ansyc dan sync dalam alur processing data pada javascript. untuk lebih jelas bisa dilihat cara pesan error yang akan kita jumpai:
```javascript
    <promise>
        panding
    </promise>
```
artinsa data yang kita fecth sudah berahsil tapi pengembalinya belum seperuna.
```javascript
    //saat request data
    getYoutubPlayList({commit},params){
        var options = {
            method: 'GET',
            url: 'https://www.googleapis.com/youtube/v3/playlistItems',
            params: {
              part: 'snippet',
              playlistId: params.playlistId,
              key: process.env.MIX_API_YOUTUBE,
              maxResults: params.maxResults,
            },
            headers: {Accept: '*/*', 'Content-Type':'application/json'}
          };

          return  new Promise((resolve,reject)=>{
            axios
              .request(options)
              .then(response =>{
                  if(response.status = 200){
                      setTimeout(()=>{
                        resolve(response.data)
                      },1000)
                  }
              })
              .catch(error =>{
                  console.log(error);
                reject(error)
              })
          })          
    },

    //saat data kita ambil
    this.PlayListHrd(this.paramasHrd)
        .then((res)=>{
            if(this.dataParsial.hrd == null){
                this.dataParsial.hrd = res.items
                console.log(this.dataParsial.hrd);
            }else{
                console.log('data hrd ada ada');
            }
        })
        .catch((err)=>{
            console.log(err);
        })

```