![image](https://user-images.githubusercontent.com/39585097/75446959-ffd1da00-599a-11ea-9c7b-0781d7dcdd9c.png)

# Sekilas Tentang

Dalam wikipedia dijelaskan CMS adalah singkatan dari Content Management System atau dalam Bahasa Indonesianya Sistem Manajemen Konten merupakan perangkat lunak yang memungkinkan seseorang untuk menambahkan dan/atau memanipulasi (mengubah) isi dari suatu situs web. Salah satu contoh CMS adalah ELGG.

Elgg adalah software jejaring sosial open source yang menyediakan komponen yang diperlukan untuk menciptakan lingkungan sosial online. ELGG menyediakan fitur blogging, microblogging, file sharing, networking, grup, dan fitur lainnya.

Elgg gratis di download dan digunakan. Elgg bisa digunakan pada platform LAMP (Linux, Apache, MySQL, dan PHP). 

# Instalasi
### Instalasi LAMP
ikuti tahap yang dideskripsikan pada https://github.com/auriza/komdat-lab/blob/master/p01.md

##### Akses vm dari host
`ssh student@localhost -p 2200`

##### Set repo
`sudo tee /etc/apt/sources.list << !` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic          main restricted universe multiverse` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic-updates  main restricted universe multiverse` <br>
`deb http://repo.apps.cs.ipb.ac.id/ubuntu bionic-security main restricted universe multiverse` <br>
`!`

##### Instal apache, mysql, php
`sudo apt update` <br>
`sudo apt install apache2 php mysql-server` <br>
`sudo apt install php-mysql php-gd php-mbstring php-xml php-curl` <br>
`sudo service apache2 restart`

Setelah tahapan installing Apache2, command berikut dapat digunakan untuk menstop, menstart ataupun enable apache2 servicenya. 
Apache2 selalu di start bersamaan dengan server boots.

`sudo systemctl stop apache2.service` <br>
`sudo systemctl start apache2.service` <br>
`sudo systemctl enable apache2.service` <br>

Untuk mengecek pengaturan Apache2, buka

# Konfigurasi
# Maintenance
# Cara Pemakaian
# Pembahasan
# Referensi
1. https://gegeriyadi.com/sistem-manajemen-konten/
2. https://en.wikipedia.org/wiki/Elgg_(software)
3. https://websiteforstudents.com/install-elgg-social-networking-engine-on-ubuntu-16-04-17-10-18-04-with-apache2-mariadb-and-php-7-2-support/
