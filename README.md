# IoT-AWS-EC2-W1

Raka Dhaneswara 
2402226133

```
Hello World!
```
## 1. Membuat akun AWS
Lakukan sign up dengan menggunakan free tier edition. Siapkan kebutuhan pendataan akun yang dibutuhkan aws.
<img width="1210" alt="Screen Shot 2022-09-06 at 10 42 52 AM" src="https://user-images.githubusercontent.com/58630970/189055655-fd2aef10-b03e-49df-8111-1397fd9c8467.png">

## 2. Buat sebuah EC2 Instance
Berikut step-step yang saya jalankan dengan UI AWS Classic Wizard:
- Buat sebuah Key Pair, EC2 > Key Pairs > Create Key Pairs
-- Disini saya menamakan keypair yang saya punya dengan _iotec2raka_
<img width="669" alt="Screen Shot 2022-09-07 at 11 44 58 AM" src="https://user-images.githubusercontent.com/58630970/189056837-182fa48e-1a48-4317-8e0c-be3e9d7dffeb.png">

- Pilih AMI yang diingikan, untuk membuatnya tetap sederhana saya memilih Basic 64-bit amazon Linux
<img width="1190" alt="Screen Shot 2022-09-07 at 11 31 24 AM" src="https://user-images.githubusercontent.com/58630970/189056796-0c0953e4-4e9b-4280-912a-61875932cad5.png">

- Pilih dengan Tipe yang kecil dengan label _free tier eligible_
<img width="1188" alt="Screen Shot 2022-09-07 at 11 32 51 AM" src="https://user-images.githubusercontent.com/58630970/189056823-71a8ae60-10c9-4e6d-85ab-f66a1182603a.png">

- Pada Security Group Options, Beri akses untuk *http* dan *https*
<img width="596" alt="Screen Shot 2022-09-08 at 9 58 33 AM" src="https://user-images.githubusercontent.com/58630970/189064020-5908fcd1-a3a4-4443-86d9-d1f4b458c01d.png">

- Klik Launch Instance
- Muncul Pop Key Pair, Arahkan kepada Key Pair yang sudah kita buat diawal (existing)

<img width="732" alt="Screen Shot 2022-09-07 at 12 02 51 PM" src="https://user-images.githubusercontent.com/58630970/189056851-6b9dce63-0cb5-4ba6-a3af-6ae6155e20b6.png">

- Sebuah Instance berhasil dibuat.
<img width="1010" alt="Screen Shot 2022-09-07 at 12 03 30 PM" src="https://user-images.githubusercontent.com/58630970/189056860-a2535b1d-90c3-4b73-aaee-b84e8b4a8d13.png">


## 3. SSH kedalam Instance yand telah dibuat
- Pertama-tama Carilah Public IP Address (DNS) yang diberikan dari instance yang sudah dibuat.

- Siapkan satu folder khusus, untuk menyimpan .pem file (key pair) yang sudah terunduh. lakukan perubahan role pada file .pem
```
[ec2-user ~]$ chmod 600 ~/[KEY_PAIR].pem
```

lalu lakukan:
```
ssh ec2-user@[YOUR_PUBLIC_DNS] -i ~/[KEY_PAIR].pem
```

- Anda telah berhasil masuk kedalam remote server ec2 aws.
<img width="815" alt="Screen Shot 2022-09-07 at 2 14 45 PM" src="https://user-images.githubusercontent.com/58630970/189060347-208f18d2-d85d-41ac-ad09-272aa42e546f.png">


## 4. Install Apache Webserver untuk menjalankan PHP

Di terminal ketikan:
```
[ec2-user ~]$ sudo yum -y install python-simplejson     # Install PHP
[ec2-user ~]$ sudo yum update                           # Cek Modul terupdate
[ec2-user ~]$ sudo yum install -y default-jre           # Install Java untuk penjagaan
[ec2-user ~]$ sudo yum install httpd                    # Install HTTPD server
```

Jalankan Apache Web Server:
```
[ec2-user ~]$ service httpd start
```

Ketikan _Public DNS_ mu pada browser. 
```
http://ec2-34-239-115-25.compute-1.amazonaws.com
```
<img width="1169" alt="Screen Shot 2022-09-08 at 10 04 06 AM" src="https://user-images.githubusercontent.com/58630970/189069097-88c7a901-b443-4708-b3f0-891381892338.png">


## 5. Install PHP untuk menjalankan WordPress

Pada Terminal, ketikan:
[ec2-user ~]$ yum install php php-mysql

Restart Apache Web Server:
[ec2-user ~]$ service httpd restart

Untuk memastikan kembali bahwa PHP sudah aktif berjalan, buat sebuah file.php sederhana sebagai berikut:
```
[ec2-user ~]$ cd /var/www/html
[ec2-user ~]$ vi test.php
```

- ketikan **i** untuk memulai edit dan masukan,
```
<?php phpinfo() ?>
```
Simpan dan keluar dari vi denga mengetikkan **:wq **

Buka Kembali Browser dan ketikan:
```
http://ec2-34-239-115-25.compute-1.amazonaws.com/test.php
```
<img width="1169" alt="Screen Shot 2022-09-08 at 11 11 30 AM" src="https://user-images.githubusercontent.com/58630970/189069024-68ab9187-fc36-466e-be5c-deaf06c7ff4a.png">

## 6. Install MySQL sebagai database

Install MySQL, ketikan:
```
[ec2-user ~]$ yum install mysql-server
```

Jalankan MySQL:
```
[ec2-user ~]$ service mysqld start
```

Create database, disini saya namakan _wprakadb_
```
[ec2-user ~]$ mysqladmin -u root create blwprakadbog
```

Berikan  _mysql_secure_installation_ pada database:
```
[ec2-user ~]$ mysql_secure_installation
```
Muncul Pertanyaan wizard, ikuti langkah berikut

```
1. Enter current password for root: Press return for none
2. Change Root Password: Y
3. New Password: Enter your new password
4. Remove anonymous user: Y
5. Disallow root login remotely: Y
6. Remove test database and access to it: Y
7. Reload privilege tables now: Y
```
Cek MySQL Version
```
mysql -v 
OR 
mysql --version
```
<img width="1280" alt="Screen Shot 2022-09-07 at 4 18 10 PM" src="https://user-images.githubusercontent.com/58630970/189069749-68fdcae3-bd81-433d-8ab8-c16124d76467.png">

jangan lupa untuk menjalankan server mysql

## 7. Install WordPress

Install WordPress, ketikan:
```
[ec2-user ~]$ cd /var/www/html
[ec2-user ~]$ wget http://wordpress.org/latest.tar.gz
```
Ekstrak file tar.gz:
```
[ec2-user ~]$ tar -xzvf latest.tar.gzcd
```

Secara Personal saya lebih nyaman me-rename folder wordpress dengan nama sub domain situs yang kita inginkan, maka:
```
[ec2-user ~]$ mv wordpress wpraka
```

Modifikasi wp-config-sample.php menjadi wp-config.php file:
```
[ec2-user ~]$ cd wpraka
[ec2-user ~]$ mv wp-config-sample.php wp-config.php
```

lalu ubah konfigurasi wp-config.php disesuaikan dengan Database yang sudah kita buat (wprakadb)
```
[ec2-user ~]$ vi wp-config.php
```

Konfigurasi:
```
define(‘DB_NAME’, ‘wprakadb’);
define(‘DB_USER’, ‘root’);
define(‘DB_PASSWORD’, ‘YOUR_PASSWORD’);
define(‘DB_HOST’, ‘localhost’);
```
<img width="182" alt="Screen Shot 2022-09-08 at 9 20 03 AM" src="https://user-images.githubusercontent.com/58630970/189069761-af76152c-6e40-4f52-ac80-5ff58c471089.png">
<img width="528" alt="Screen Shot 2022-09-08 at 9 34 41 AM" src="https://user-images.githubusercontent.com/58630970/189069766-9cce0677-4aaf-4862-be93-bc098725ccd7.png">

- Klik :wq untuk keluar dan menyimpan perubahan


## 8. Konfigurasi Wordpress yang telah dibuat
- Sesuaikan Konfigurasi Wordpress dengan kebutuhan:
<img width="769" alt="Screen Shot 2022-09-08 at 11 12 27 AM" src="https://user-images.githubusercontent.com/58630970/189086573-8303b7ef-b632-4c17-b8f8-64eb539d225e.png">

- Anda telah berhasil masuk ke Dashboard Admin Wordpress anda:
<img width="1134" alt="Screen Shot 2022-09-08 at 11 27 24 AM" src="https://user-images.githubusercontent.com/58630970/189086603-e7e3c8d2-954d-4e77-bbf0-9f4d4be4910c.png">

- Tampilan Home Page Wordpress berhasil dibuat:
<img width="1280" alt="Screen Shot 2022-09-08 at 1 36 15 PM" src="https://user-images.githubusercontent.com/58630970/189086617-5df311f5-8cac-482a-aa43-8dbfdd8612fb.png">

Buka kembali browser dan akses situsmu:
**http://ec2-50-17-15-27.compute-1.amazonaws.com/wpraka**

That's it! 💯 💯 💯 

Terima Kasih

# References
- _https://www.rosehosting.com/blog/how-to-configure-wordpress-to-use-a-remote-database/_
- _https://stackoverflow.com/questions/53712228/no-package-msyql-server-available_
- _https://www.hostinger.com/tutorials/how-to-install-mysql-on-centos-7_
- _https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-centos-7_
- _https://stackoverflow.com/questions/40620719/this-site-can-t-be-reached-amazon-ec2_
- _https://computingforgeeks.com/how-to-install-php-7-4-on-centos-7/_
