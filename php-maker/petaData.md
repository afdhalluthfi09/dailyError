# Cara Memvisualisasikan data dengan peta

untuk melakukan visual data ada beberapa requirement :
    - library d3 js untuk melakukan visual dan bind data ke dalam aplikasi
    - topojson : untuk melakukan penerjamahan data peta 
    - html
    -qgis untuk mengkostum data peta

langkah pertama buat canvasnya dengan creat svg:
```javascript

    //buat svg dengan select id
     let svg = d3.select('#peta')
                 .append("svg")
                 .attr("width",500)
                 .attr("height",500)
    /* 
        persiapan kebutihan visual peta:
        melakuakn pembuatan garis dengan metode geoMercator(khusus dataran yang tropikal)
        masukan kedalam path
     */
    //  setp 1
    let projection = d3.geoMercator()
                        .center([]) //posisi peta lokasi
                        .scale() //zoom peta
                        .translate () //pososisi peta by line_x dan line_y
    //  step 2
    let path = d3.geopath()
                    .projection(projection) //masukan variabel projection
    // masukin data peta yang bertipe json atau topojson
    d3.queue()
        .defer(d3.json,'peta.json')
        .await(ready);
    
    function ready(err, data){
        console.log(data) //pastikan data sudah berhasil diload
        let peta = topojson.feature(data, data.objects.collection).features
        console.log(peta) //pastikan data sudah berhasil dipilih
    }

    