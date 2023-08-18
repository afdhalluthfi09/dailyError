# PHP

###### swicth php dengan phpbrew

1. peratama pastikan sudah menginstall require yang diminta lihat di situs resmi mereka install [requrier phpbrew](https://github.com/phpbrew/phpbrew/wiki/Requirement "https://github.com/phpbrew/phpbrew/wiki/Requirement"):
   version ubuntu 22.04

   ```php
   sudo apt-get install \
     build-essential \
     libbz2-dev \
     libreadline-dev \
     libsqlite3-dev \
     libcurl4-gnutls-dev \
     libzip-dev \
     libssl-dev \
     libxml2-dev \
     libxslt-dev \
     php8.1-cli \
     php8.1-bz2 \
     php8.1-xml \
     pkg-config
   ```
2. lakukan instalasi dengan menggunakan perunta di bawah ini:

   ```php
   //step 1
   curl -L -O https://github.com/phpbrew/phpbrew/releases/latest/download/phpbrew.phar

   //step 2
   chmod +x phpbrew.phar

   //step 3
   # Move the file to some directory within your $PATH
   sudo mv phpbrew.phar /usr/local/bin/phpbrew
   ```
3. selanjutnya tambahkan ini di lisesni .bashr atau .zhsrc:

   `phpbrew init`

   `[[ -e ~/.phpbrew/bashrc ]] && source ~/.phpbrew/bashrc`
4. jika sudah silahkan test dengan menetikan perintah phpbrew.
5. untuk lebih lanjut lihat dokumentasinya di[ phpbrew](https://github.com/phpbrew/phpbrew "https://github.com/phpbrew/phpbrew")
