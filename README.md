![image](https://user-images.githubusercontent.com/39585097/75446959-ffd1da00-599a-11ea-9c7b-0781d7dcdd9c.png)

# Sekilas Tentang

Dalam wikipedia dijelaskan CMS adalah singkatan dari Content Management System atau dalam Bahasa Indonesianya Sistem Manajemen Konten merupakan perangkat lunak yang memungkinkan seseorang untuk menambahkan dan/atau memanipulasi (mengubah) isi dari suatu situs web. Salah satu contoh CMS adalah ELGG.

Elgg adalah software jejaring sosial open source yang menyediakan komponen yang diperlukan untuk menciptakan lingkungan sosial online. ELGG menyediakan fitur blogging, microblogging, file sharing, networking, grup, dan fitur lainnya.

Elgg gratis di download dan digunakan. Elgg bisa digunakan pada platform LAMP (Linux, Apache, MySQL, dan PHP). 

# Instalasi
### Instalasi LAMP
ikuti tahap yang dideskripsikan pada https://github.com/auriza/komdat-lab/blob/master/p01.md

##### Akses vm dari host
> `ssh student@localhost -p 2200`

##### Set repo
> `sudo tee /etc/apt/sources.list << !` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic          main restricted universe multiverse` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic-updates  main restricted universe multiverse` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic-security main restricted universe multiverse` <br>
`!`

##### Instal apache, mysql, php
> `sudo apt update` <br>
`sudo apt install apache2 php mysql-server` <br>
`sudo apt install php-mysql php-gd php-mbstring php-xml php-curl` <br>
`sudo service apache2 restart`

Setelah tahapan installing Apache2, command berikut dapat digunakan untuk menstop, menstart ataupun enable apache2 servicenya. <br><br>
Apache2 selalu di start bersamaan dengan server boots. <br>
> `sudo systemctl stop apache2.service` <br>
`sudo systemctl start apache2.service` <br>
`sudo systemctl enable apache2.service` <br>
<br>
Open localhost:8000 <br>
<br>

![image](https://user-images.githubusercontent.com/47512858/75656088-cdbfc100-5c95-11ea-8575-d7f45d52a7b6.png)

Untuk mengetes apakah instalasi mysql berhasil, gunakan command dibawah untuk login ke mysql server. <br>
> `sudo mysql -u root` <br>

### Install PHP 7.2 and Related Modules
Run command dibawah ini untuk menambah repositori pihak ketiga untuk di upgrade ke PHP. <br>
> `sudo apt-get install software-properties-common` <br>
`sudo add-apt-repository ppa:ondrej/php` <br>

Setelah itu, run command di bawah ini untuk menginstall PHPnya beserta modul-modul terkait. <br>
> `sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-sqlite3 php7.2-curl php7.2-intl php7.2-mbstring php7.2-xmlrpc php7.2-mysql php7.2-gd php7.2-xml php7.2-cli php7.2-zip` <br>

Setelah menginstall PHP, run command di bawah ini untuk membuka default config PHP file untuk Apache2. <br>
> `sudo nano /etc/php/7.2/apache2/php.ini`<br>

Lalu ubah beberapa line di bawah ini dalam file tersebut kemudian simpan. Nilai-nilai didalam command dibawah adalah nilai yang tepat untuk digunakan dalam setting environment kita.<br>
> `file_uploads = On`<br>
`allow_url_fopen = On`<br>
`short_open_tag = On`<br>
`memory_limit = 256M`<br>
`upload_max_filesize = 100M`<br>
`max_execution_time = 360`<br><br>
`date.timezone = America/Chicago`<br>

Setelah melakukan semua tahapan diatas, save file dan keluar.

### Restart Apache2
Untuk restart Apache2, run command dibawah ini.<br>
> `sudo systemctl restart apache2.service`

Untuk mencoba PHP setting dengan apache2, buat file **phpinfo.php** dalam direktori root apache2 dengan menjalankan command dibawah ini<br>
> `sudo nano /var/www/html/phpinfo.php`

Kemudian ketikkan content di bawah dalam file tersebut kemudian simpan.<br>
> `<?php phpinfo( ); ?>`

lalu telusuri di hostname server /**phpinfo.php**<br>
**localhost:8000/phpinfo.php**<br>

![image](https://user-images.githubusercontent.com/47512858/75656199-0495d700-5c96-11ea-9345-36cb5dd1cdcf.png)

### Create Magento Database
Untuk login ke database server mysql jalankan command dibawah ini.<br>
> `sudo mysql -u root -p`

Kemudian buat database bernama elgg.<br>
> `CREATE DATABASE elgg;`

Setelah itu buat nama database user sebagai elgguser dengan password yang baru.<br>
> `CREATE USER 'elgguser'@'localhost' IDENTIFIED BY 'student';`

Kemudian beri akses penuh untuk penggunanya ke database ini.<br>
> `GRANT ALL PRIVILEGES ON elgg.* TO 'elgguser'@'localhost'`<br>
`IDENTIFIED BY 'user_password_here' WITH GRANT OPTION;`

Setelah itu simpan perubahan dan keluar.<br>
> `FLUSH PRIVILEGES;`<br>
`EXIT;`

### Download and Install Elgg CMS

Jalankan command dibawah ini untuk mendownload content terbaru dari  ELGG CMS , kemudian unzip file hasil download tersebut kemudian pindahkan konten ke *default* direktori root Apache2. <br>
> `cd /tmp && wget https://elgg.org/download/elgg-2.3.7.zip
unzip elgg-2.3.7.zip` <br>
`sudo mv elgg-2.3.7 /var/www/html/elgg` <br>

Buat direktori data elgg untuk menyimpan konten data. <br>
> `sudo mkdir -p /var/www/html/elgg/data`

Kemudian jalankan command di bawah untuk mengubah izin folder root.<br>
> `sudo chown -R www-data:www-data /var/www/html/elgg/`<br>
`sudo chmod -R 755 /var/www/html/elgg/`

### Configure Apache2 Elgg CMS Site
Setelah itu, konfigurasikan file konfigurasi Apache2 untuk Elgg CMS. File ini akan mengontrol bagaimana pengguna mengakses konten Elgg CMS. Jalankan perintah di bawah ini untuk membuat file konfigurasi baru bernama **elgg.conf**.<br>
> `sudo nano /etc/apache2/sites-available/elgg.conf`

Kemudian copy dan paste konten dibawah ini kedalam file tersebut kemudian simpan. Kemudian ganti line yang di highlight dengan domain name milik kamu sendiri dan lokasi direktori rootnya.<br>
> `<VirtualHost *:80>`<br>
     `ServerAdmin admin@example.com`<br>
     `DocumentRoot /var/www/html/elgg`<br>
     `ServerName localhost:8000`<br>
<br>
     `<Directory /var/www/html/elgg/>`<br>
          `Options FollowSymlinks`<br>
          `AllowOverride All`<br>
          `Require all granted`<br>
     `</Directory>`<br>
<br>
     `ErrorLog ${APACHE_LOG_DIR}/error.log`<br>
     `CustomLog ${APACHE_LOG_DIR}/access.log combined`<br>
<br>
`</VirtualHost>`<br>

Simpan file kemudian keluar.<br>

Setelah mengkonfigurasi VirtualHost diatas, mengaktifkannya dengan cara menjalankan command setelah ini.<br>

### Enable the Elgg CMS Site and Rewrite Module

Setelah mengkonfigurasi VirtualHost diatas, cara mengaktifkannya dengan cara menjalankan command dibawah, lalu restart Apache2 sever.<br>
> `sudo a2ensite elgg.conf`<br>
`sudo a2enmod rewrite`<br>
`sudo systemctl restart apache2.service`<br>

kemudian, open browser dan pergi ke url [http://localhost:8000/elgg](http://localhost:8000/elgg) dan lanjutkan instalasinya lalu kalian akan melihat laman instalasi elgg kemudian tekan next untuk melanjutkan.

**http://localhost:8000/elgg**

![image](https://user-images.githubusercontent.com/47512858/75656304-4292fb00-5c96-11ea-9392-08b42d5e98ae.png)<br><br>

Jika semua elemen dalam requirement check sudah benar maka dia akan berwarna hijau dan tekan next.<br><br>
![image](https://user-images.githubusercontent.com/47512858/75656374-5dfe0600-5c96-11ea-966d-2a69cc25cce2.png)<br><br>

Lalu pada tahap instalasi database silahkan isi requirement yang ada sesuai dengan apa yang kalian buat sebelumnya. Kemudian tekan next.<br><br>
![image](https://user-images.githubusercontent.com/47512858/75656425-78d07a80-5c96-11ea-817a-025a33bc72be.png)
![image](https://user-images.githubusercontent.com/47512858/75656449-8c7be100-5c96-11ea-8c65-1e396350d482.png)
![image](https://user-images.githubusercontent.com/47512858/75656497-a7e6ec00-5c96-11ea-8cdd-77a081c612b1.png)<br><br>

Setelah semua step selesai, buat admin account maka proses pembuatan akun pada elgg selesai.<br><br>
![image](https://user-images.githubusercontent.com/47512858/75656533-bfbe7000-5c96-11ea-8ac4-5eb85f3923b7.png)
![image](https://user-images.githubusercontent.com/47512858/75656582-d664c700-5c96-11ea-9b56-f42735ea1a43.png)<br><br>

Kemudian masuk ke laman setting dashboard, isi sesuaikan dengan keperluan kalian. <br><br>
![image](https://user-images.githubusercontent.com/47512858/75656637-f300ff00-5c96-11ea-8cab-098ec3558c65.png)
![image](https://user-images.githubusercontent.com/47512858/75656669-03b17500-5c97-11ea-898c-b0be10dafae5.png)
![image](https://user-images.githubusercontent.com/47512858/75656714-1b88f900-5c97-11ea-9319-1f104f4fd654.png)
![image](https://user-images.githubusercontent.com/47512858/75656745-30658c80-5c97-11ea-82b4-17c51aa65c78.png)
![image](https://user-images.githubusercontent.com/47512858/75656775-44a98980-5c97-11ea-83ce-b7ca050e8222.png)
![image](https://user-images.githubusercontent.com/47512858/75656821-660a7580-5c97-11ea-94a7-e06c4befb859.png)
![image](https://user-images.githubusercontent.com/47512858/75656924-acf86b00-5c97-11ea-98dc-da4b592bfd78.png)
![image](https://user-images.githubusercontent.com/47512858/75656969-c39ec200-5c97-11ea-8386-be4eec06678c.png)
![image](https://user-images.githubusercontent.com/47512858/75657007-d5806500-5c97-11ea-9620-bd6bb132eaae.png)
![image](https://user-images.githubusercontent.com/47512858/75657041-eaf58f00-5c97-11ea-9fbd-907045c0cc44.png)
![image](https://user-images.githubusercontent.com/47512858/75657071-f9dc4180-5c97-11ea-9520-3982fcfb640f.png)
![image](https://user-images.githubusercontent.com/47512858/75657103-111b2f00-5c98-11ea-8937-c67878347e44.png)
![image](https://user-images.githubusercontent.com/47512858/75657153-298b4980-5c98-11ea-9306-66371ac6bcc7.png)
![image](https://user-images.githubusercontent.com/47512858/75657210-4162cd80-5c98-11ea-9804-09bef8eaf9db.png)

![image](https://user-images.githubusercontent.com/47512858/75657253-550e3400-5c98-11ea-83a1-d0528824d48b.png)
![image](https://user-images.githubusercontent.com/47512858/75657275-65beaa00-5c98-11ea-9b38-f96eee7b4697.png)<br><br>
Berikut tampilan front page laman yang telah dibuat :<br><br>
![image](https://user-images.githubusercontent.com/47512858/75657330-8be44a00-5c98-11ea-9c93-ad003ea0bce4.png)
![image](https://user-images.githubusercontent.com/47512858/75657366-9d2d5680-5c98-11ea-9a23-6e67f9218c98.png)
![image](https://user-images.githubusercontent.com/47512858/75657385-aa4a4580-5c98-11ea-84fe-5289ae1339aa.png)
![image](https://user-images.githubusercontent.com/47512858/75657415-bd5d1580-5c98-11ea-974e-8c8159fe6f2c.png)
![image](https://user-images.githubusercontent.com/47512858/75657460-d960b700-5c98-11ea-97da-faaf99a672ff.png)
![image](https://user-images.githubusercontent.com/47512858/75657491-ee3d4a80-5c98-11ea-94ff-342435e7979b.png)
![image](https://user-images.githubusercontent.com/47512858/75657519-fdbc9380-5c98-11ea-9b93-cb9e3105415d.png)
![image](https://user-images.githubusercontent.com/47512858/75657570-13ca5400-5c99-11ea-8ac7-0041a9c17711.png)


# Konfigurasi
# Maintenance
# Cara Pemakaian
# Pembahasan
# Referensi
1. https://gegeriyadi.com/sistem-manajemen-konten/
2. https://en.wikipedia.org/wiki/Elgg_(software)
3. https://websiteforstudents.com/install-elgg-social-networking-engine-on-ubuntu-16-04-17-10-18-04-with-apache2-mariadb-and-php-7-2-support/
4. https://elgg.org/
