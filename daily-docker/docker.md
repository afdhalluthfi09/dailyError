# Daily Docker

dokementasi untuk melakukan penyetingan docker saat dokuemntasi di buat, docker yang di gunakan docker deksop versi windows dan mackbook

###### Requier Win 10

pastikan di windows  sudah memenuhi ini:

<ul>
    <li>WSL 2 update</li>
    <li>File Installer Dokcer Dekstop</li>
</ul>
masalah yang terjadi jika WSL 2 ini belum terinstall atau belum update maka akan terjadi crash pada sistem operasi yang mengakibatkan blank screen pada kasus di sistem operasi win 10 saya.

###### Require Mac Os

pastikan saat mengunduh versi dokcer haru s sesuai dengan chip dari mac os-nya agar tidak terjadi crush..

ketika semua di atas terpenuhi maka bisa langsung menginstall dockernya

###### Check Docker Version

jalankan perintah ini di terminal

`docker version`

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

###### Docker Image

merupakan Installer (sebutan di beberapa Operasi System) untuk packge yang kita butuhkan, dan docker image tersebut juga memiliki dependecy untuk bisa menjalankannya, dan untuk bisa medpatakan docker image yang kita butuhkan kita cukup ke salah satu diantara penyedia docker image seperti Docker Hub

<li>cara melihat docker image yang ada di operasi system kita dengan perintah ini :</li>

`docker image ls`

hasilnya

`REPOSITORY   TAG       IMAGE ID   CREATED   SIZE`

###### Cara Download Docker Image

untuk menginstall package yang kita butuhkan di docker kita cukup menggunakan mengguanakn kata perinta seperti ini:

`docker pull name_image:version`

###### Cara Hapus/Uninstall Docker Image

untuk menghapusnya sendiri cukup menggunakan cara :

`docker image rm name_image:version`

###### Docker Container

[butuh revisi belum valid] docker conatiner sendiri adalah sebuah wadah aplikasi itu sendiri dimana kita akan menggunakan docker image yang kita dowload sebagai bagian part dari aplikasi kita, contohnya:`<br/>`
"Kita akan membangun aplikasi web toko online kalo menggunakan docker kita cukup mendownload kebutahn aplikasi yang kita bangun misal dalam kasus ini kita butuh framewk laravel dan database maka sebelum membuat containernya kita butuh dependency docker image seperti composer,php,mysql,nodejs, yang nantinya kita gunakan untuk membangun aplikasinya di container-nya"

###### Melihat Docker Container

jiak menambhakn -a artinya kita ingin melihat beberapa container yang statusnya aktive atau tidak,jika tidak menggunakna -a artinya default melihat container yang active

`docker container ls -a`

hasilnya

`CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES`

###### Membuat Docker Container

`docker container create --name namaContainer name_image:version`

###### Menjalankan Docker Container

untuk menjalankan bisa manggil dengan id atau nama container-nya

`docker container start idContainer/nameContainer`

###### Menjalankan docker di ubuntu 22.

Tentu! Berikut adalah langkah-langkah untuk menginstal Docker di Ubuntu 22.04 dan mengatur lingkungan Docker untuk pengembangan dengan Laravel dan Vue.js.

Langkah 1: Instal Docker di Ubuntu 22.04

1. **Perbarui Paket APT:**

```
sudo apt update
```

2. **Instal Paket-Paket Prasyarat:**

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

3. **Tambahkan Kunci GPG Docker:**

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4. **Tambahkan Repositori Docker:**

```
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. **Perbarui Kembali Paket APT dan Instal Docker:**

```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

6. **Tambahkan Pengguna Anda ke Grup "docker" (Opsional, namun dianjurkan):**

```
sudo usermod -aG docker $USER
```

7. **Mulai dan Aktifkan Docker:**

```
sudo systemctl enable docker
sudo systemctl start docker
```

Langkah 2: Instal Docker Compose

1. **Unduh Binary Docker Compose:**

```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. **Beri Izin Eksekusi pada Binary Docker Compose:**

```
sudo chmod +x /usr/local/bin/docker-compose
```

Langkah 3: Instal Laravel dan Vue.js di Lingkungan Docker

1. **Buat Direktori Proyek:**

```
mkdir laravel-vue-app
cd laravel-vue-app
```

2. **Buat Berkas `Dockerfile` untuk Laravel:**
   Buat berkas bernama `Dockerfile` di dalam direktori proyek dengan konten berikut:

```
# Gunakan gambar PHP sebagai dasar
FROM php:8.0-fpm

# Instal dependensi
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip

# Instal ekstensi PHP yang dibutuhkan
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd pdo pdo_mysql

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Setel direktori kerja
WORKDIR /var/www/html

# Salin file composer.json dan composer.lock
COPY composer.json composer.lock /var/www/html/

# Install dependensi PHP
RUN composer install

# Expose port 9000
EXPOSE 9000
```

3. **Buat Berkas `docker-compose.yml`:**
   Buat berkas `docker-compose.yml` di dalam direktori proyek dengan konten berikut:

```yaml
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    image: laravel-vue-app
    ports:
      - 9000:9000
    volumes:
      - .:/var/www/html
```

4. **Buat Proyek Laravel:**

```
docker-compose run --rm app composer create-project --prefer-dist laravel/laravel .
```

5. **Tambahkan Dependensi untuk Vue.js:**
   Dalam berkas `package.json`, tambahkan dependensi Vue.js:

```json
"dependencies": {
    // ...
    "vue": "^2.6.14"
},
```

6. **Instal Dependensi:**

```
docker-compose run --rm app npm install
```

7. **Buat Komponen Vue.js:**
   Buat komponen Vue.js dalam direktori `resources/js/components`.
8. **Aktifkan Laravel Mix untuk Vue.js:**
   Edit berkas `webpack.mix.js` untuk memasukkan komponen Vue.js:

```javascript
mix.js('resources/js/app.js', 'public/js')
    .vue();
```

9. **Jalankan Aplikasi:**

```
docker-compose up -d
```

Dengan langkah-langkah di atas, Anda telah menginstal Docker, Docker Compose, Laravel, dan Vue.js dalam lingkungan Docker pada Ubuntu 22.04. Anda sekarang dapat mengakses aplikasi Laravel dan Vue.js Anda melalui browser dengan alamat `http://localhost:9000`.

Pastikan untuk mengacu pada dokumentasi resmi Docker, Laravel, dan Vue.js untuk informasi lebih lanjut tentang pengaturan dan pengembangan lebih lanjut.

###### Setting Docker 2025

pada tahun ini menggunakan konsep satu racikan docker menjalankan banyak aplikasi, skenarionya seperti ini, sebut aja ex: project sekolahskill dimana di dalamnya ada api-skill,sekolahskill,manage-sekolahskill disana nantinya juga ada docker-compose.yml
project-sekolahskill/
   ->api-skill/
   ->sekolahskill/
   ->manage-sekolahskill/
   ->docker-compose.yml
tujuanya agar aplikasi berjalan dengan satu docker sehingga satu komunikasi,satu flow pengembangan,dan maintenance yang seragam adapun isinya docker compse yml-nya begini:

```yml
services:
  # Database Terpisah
  db:
    image: mysql:8.0
    container_name: skill-db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: skill
      MYSQL_ROOT_PASSWORD: root
    ports:
      - "3306:3306"
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - skill-network

  # Backend (Laravel)
  api:
    build:
      context: ./api-skill
      dockerfile: docker/php/Dockerfile
    container_name: api-skill
    volumes:
      - ./api-skill:/var/www
    depends_on:
      - db
    networks:
      - skill-network
  nginx:
    image: nginx:stable-alpine
    container_name: nginx-skill
    ports:
      - "8080:80"
    volumes:
      - ./api-skill:/var/www
      - ./api-skill/docker/nginx/default.conf:/etc/nginx/conf.d/default.conf # 4. Arahkan ke config di subfolder
    depends_on:
      - api
    networks:
      - skill-network

  # Frontend User
  # frontend:
  #   build:
  #     context: ./sekolahskill
  #   container_name: frontend-user
  #   ports:
  #     - "3000:3000"
  #   networks:
  #     - skill-network

  # Frontend Admin
  # admin:
  #   build:
  #     context: ./manage-sekolahskill
  #   container_name: frontend-admin
  #   ports:
  #     - "3001:3000"
  #   networks:
  #     - skill-network

networks:
  skill-network:
    driver: bridge

volumes:
  db-data:
```

dan untuk tiap **dockerfilenya** ada di dalam tiap subfolder itu sendiri saat ini baru berjalan ada di sub folde api-skill:
docker/php/Dockerfile

```docker
FROM php:8.1-fpm-bookworm

# System dependencies (sekali saja)
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libicu-dev \
    zip \
    unzip \
    curl \
    git \
    && rm -rf /var/lib/apt/lists/*

# PHP extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
    pdo_mysql \
    mbstring \
    exif \
    pcntl \
    bcmath \
    gd \
    zip \
    intl

# Redis
RUN pecl install redis \
    && docker-php-ext-enable redis

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www
```

docker/nginx/default.conf

```conf
server {
    listen 80;
    index index.php index.html;
    root /var/www/public;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass api:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
    }


    location ~ /\.ht {
        deny all;
    }
}
```
