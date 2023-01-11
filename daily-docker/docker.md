# Daily Docker
dokementasi untuk melakukan penyetingan docker saat dokuemntasi di buat, docker yang di gunakan docker deksop versi windows dan mackbook 

## Requier Win 10
pastikan di windows  sudah memenuhi ini:
 <ul>
    <li>WSL 2 update</li>
    <li>File Installer Dokcer Dekstop</li>
</ul>
masalah yang terjadi jika WSL 2 ini belum terinstall atau belum update maka akan terjadi crash pada sistem operasi yang mengakibatkan blank screen pada kasus di sistem operasi win 10 saya.

## Require Mac Os
pastikan saat mengunduh versi dokcer haru s sesuai dengan chip dari mac os-nya agar tidak terjadi crush..

ketika semua di atas terpenuhi maka bisa langsung menginstall dockernya

## Check Docker Version
jalankan perintah ini di terminal
```terminal
    docker version
```
<li>hasilnaya</li>

```terminal
Client:
 Cloud integration: v1.0.29
 Version:           20.10.21
 API version:       1.41
 Go version:        go1.18.7
 Git commit:        
 Built:             Tue Oct 25 18:01:18 2022
 OS/Arch:           darwin/arm64
 Context:           default
 Experimental:      true

Server: Docker Desktop 4.15.0 (93002)
 Engine:
  Version:          20.10.21
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.7
  Git commit:       
  Built:            Tue Oct 25 17:59:41 2022
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.6.10
  GitCommit:        
 runc:
  Version:          1.1.4
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        
```
## Docker Image
merupakan Installer (sebutan di beberapa Operasi System) untuk packge yang kita butuhkan, dan docker image tersebut juga memiliki dependecy untuk bisa menjalankannya, dan untuk bisa medpatakan docker image yang kita butuhkan kita cukup ke salah satu diantara penyedia docker image seperti Docker Hub
<li>cara melihat docker image yang ada di operasi system kita dengan perintah ini :</li>

```terminal
    docker image ls
```
<li>hasilnya :</li>

```terminal
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```
## Cara Download Docker Image
untuk menginstall package yang kita butuhkan di docker kita cukup menggunakan mengguanakn kata perinta seperti ini:
```terminal
docker pull name_image:version
```
## Cara Hapus/Uninstall Docker Image
untuk menghapusnya sendiri cukup menggunakan cara :
```terminal
docker image rm name_image:version
```
## Docker Container
[butuh revisi belum valid] docker conatiner sendiri adalah sebuah wadah aplikasi itu sendiri dimana kita akan menggunakan docker image yang kita dowload sebagai bagian part dari aplikasi kita, contohnya:<br/>
"Kita akan membangun aplikasi web toko online kalo menggunakan docker kita cukup mendownload kebutahn aplikasi yang kita bangun misal dalam kasus ini kita butuh framewk laravel dan database maka sebelum membuat containernya kita butuh dependency docker image seperti composer,php,mysql,nodejs, yang nantinya kita gunakan untuk membangun aplikasinya di container-nya"
## <li> Melihat Docker Container </li>
jiak menambhakn -a artinya kita ingin melihat beberapa container yang statusnya aktive atau tidak,jika tidak menggunakna -a artinya default melihat container yang active
```terminal
docker container ls -a
```
<u>hasilnya</u>

```terminal
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
## <li>Membuat Docker Container</li>
```terminal
docker container create --name namaContainer name_image:version
```
## <li>Menjalankan Docker Container</li>
untuk menjalankan bisa manggil dengan id atau nama container-nya
```terminal
docker container start idContainer/nameContainer
```

