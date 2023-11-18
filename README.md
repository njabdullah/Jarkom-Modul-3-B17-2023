# Jarkom-Modul-3-B17-2023

## Anggota Kelompok
| Nama                       | NRP        |
| -------------------------- | ---------- |
| Yohanes Teguh Ukur Ginting | 5025211179 |
| Abdullah Nasih Jasir       | 5025211111 |

## Topology

## Soal 1
>Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

Di Heiter

    nano /etc/bind/named.conf.local

    zone "riegel.canyon.b17.com" {
    	type master;
    	file "/etc/bind/jarkom/riegel.canyon.b17.com";
    };
    
    zone "granz.channel.b17.com" {
    	type master;
    	file "/etc/bind/jarkom/granz.channel.b17.com";
    };

    mkdir /etc/bind/jarkom
    cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.b17.com
    cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.b17.com

    nano /etc/bind/jarkom/riegel.canyon.b17.com
    ganti nama
    ganti ip (10.17.4.4) // IP Frieren

    nano /etc/bind/jarkom/granz.channel.b17.com
    ganti nama
    ganti ip (10.17.3.4) // IP Lawine

    service bind9 restart

![Screenshot 2023-11-15 224945](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/f931195d-3bb8-4ce6-abf2-fdbd589142b7)

Testing Di Frieren
    
    echo nameserver 10.17.1.3 > /etc/resolv.conf
  	ping riegel.canyon.b17.com

Testing Di Lawine
  
    echo nameserver 10.17.1.3 > /etc/resolv.conf
  	ping granz.channel.b17.com

## Soal 2
>Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)

Di Himmel

    apt-get update
    apt-get install isc-dhcp-server -y
    
    nano /etc/default/isc-dhcp-server
    masukan “INTERFACES=”eth0”
    
    nano /etc/dhcp/dhcpd.conf
    
    masukkan
    subnet 10.17.1.0 netmask 255.255.255.0 {}
    subnet 10.17.2.0 netmask 255.255.255.0 {}

    subnet 10.17.3.0 netmask 255.255.255.0 {
        range 10.17.3.16 10.17.3.32;
        range 10.17.3.64 10.17.3.80;
        option routers 10.17.3.1;
        option broadcast-address 10.17.3.255;
        option domain-name-servers 10.17.1.3;
        default-lease-time 180;
        max-lease-time 5760;
    }

## Soal 3
>Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)

Lanjutan Nomor 2<br>
Di Himmel

    nano /etc/dhcp/dhcpd.conf
    
    subnet 10.17.4.0 netmask 255.255.255.0 {
        range 10.17.4.12 10.17.4.20;
        range 10.17.4.160 10.17.4.168;
        option routers 10.17.4.1;
        option broadcast-address 10.17.4.255;
        option domain-name-servers 10.17.1.3;
        default-lease-time 720;
        max-lease-time 5760;
    }

    service isc-dhcp-server restart
    service isc-dhcp-server status

## Soal 4
>Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)

Pada dasarnya, saat mengatur subner, seluruh Client sudah diatur untuk terhubung ke Heiter<br>
Di Himmel

    subnet 10.17.3.0 netmask 255.255.255.0 {
        ...
        option broadcast-address 10.17.3.255;
        option domain-name-servers 10.17.1.3;
        ...
    }
    
    subnet 10.17.4.0 netmask 255.255.255.0 {
        ...
        option broadcast-address 10.17.4.255;
        option domain-name-servers 10.17.1.3;
        ...
    }

Di Aura

    apt-get update
    apt-get install isc-dhcp-relay -y 
    service isc-dhcp-relay start
    
    nano /etc/default/isc-dhcp-relay
    SERVERS=”10.17.1.2”
    
    nano /etc/sysctl.conf
    net.ipv4.ip_forward=1
    
    service isc-dhcp-relay restart

Agar terhubung ke internet, maka perlu dilakukan setting sebagai berikut<br>
Di Heiter

    echo nameserver 192.168.122.1 >> /etc/resolv.conf
    apt-get update
    apt-get install bind9 -y
    
    nano /etc/bind/named.conf.options
    comment // dnssec-validation auto;
    
    tambahkan
    forwarders {
        192.168.122.1;
    };
    allow-query{any;};
    
    service bind9 restart

## Soal 5
>Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)

Pada dasarnya, ini sudah dilakukan penyetingan saat mengerjakan nomor 2

    subnet 10.17.3.0 netmask 255.255.255.0 {
        ...
        default-lease-time 180;
        max-lease-time 5760;
    }
    
    subnet 10.17.4.0 netmask 255.255.255.0 {
        ...
        default-lease-time 720;
        max-lease-time 5760;
    }


## Soal 6
>Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

Di PHP Worker (Lawine, Linie, Lugner)

    apt-get update
    apt install nginx php php-fpm -y
    apt-get install wget
    apt-get install unzip
    
    wget --no-check-certificate 'https://drive.usercontent.google.com/download?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download&authuser=0&confirm=t&uuid=0e499712-8150-42d4-a474-b29dfb026ab6&at=APZUnTVBse4ducwDDntmAkLSWB1_:1699949521984' -O  granz.channel.a17.com
    
    unzip granz.channel.a17.com
    
    cp -r modul-3/ /var/www

    rm -r modul-3
    
    nano /etc/nginx/sites-available/jarkom

  	tambahkan
  	server {
       	listen 80;
       	root /var/www/modul-3;
       	index index.php index.html index.htm;
       	server_name _;
       	location / {
       			try_files $uri $uri/ /index.php?$query_string;
       	}
       	# pass PHP scripts to FastCGI server
       	location ~ \.php$ {
       	include snippets/fastcgi-php.conf;
       	fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
       	}
       	location ~ /\.ht {
       			deny all;
       	}
       	error_log /var/log/nginx/jarkom_error.log;
       	access_log /var/log/nginx/jarkom_access.log;
   	}

    ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
  	rm -r /etc/nginx/sites-enabled/default

    service nginx reload
    service nginx restart

    service php7.3-fpm start
    service php7.3-fpm restart

Di Semua Client (Saya melakukan setting di Revolte)

    apt-get update
    apt install lynx -y

    Testing
    lynx granz.channel.b17.com

## Soal 7
>Kepala suku dari Bredt Region memberikan resource server sebagai berikut:<br>
a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.<br>
b. Linie, 2GB, 2vCPU, dan 50 GB SSD.<br>
c. Lugner 1GB, 1vCPU, dan 25 GB SSD.<br>
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

Di Eisen

    echo nameserver 192.168.122.1 > /etc/resolv.conf
    apt-get update
    apt install nginx php php-fpm -y
    
    nano /etc/nginx/sites-available/lb-jarkom
     
    Tambahkan
    upstream myweb {
           server 10.17.3.4;
           server 10.17.3.5;
           server 10.17.3.6;
    }

    server {
            listen 80;
            server_name granz.channel.a17.com;
            location / {
            proxy_pass http://myweb;
            }
    }
    
    ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
    rm -r /etc/nginx/sites-enabled/default
    service nginx start
    
Di Heiter
    
    nano /etc/bind/jarkom/granz.channel.b17.com
    ubah IP arahin ke Load Balancer 10.17.2.3
    
    service bind9 restart

Di Revolte
    
    apt-get install apache2
    service apache2 start
    ab -n 1000 -c 100 http://www.granz.channel.b17.com/ 

![Screenshot 2023-11-16 170210](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/2861ebe1-2f7e-4d59-af89-95783a782660)

![Screenshot 2023-11-16 170221](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/eab0c912-6a9e-4677-901f-ffe29fa7f51d)


## Soal 8
>Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:<br>
a. Nama Algoritma Load Balancer<br>
b. Report hasil testing pada Apache Benchmark<br>
c. Grafik request per second untuk masing masing algoritma.<br> 
d. Analisis<br>

Grafik request per second untuk masing masing algoritma
![Screenshot 2023-11-18 010015](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/af594523-256c-4142-b9ea-520fe616ca1e)

Analisis

        Analisis Algoritma Load Balancer<br>
        
        List Request per Second<br>
        Round Robin		: 495.82 req/s<br> 
        IP Hash		    : 669.21 req/s<br>
        Least Connection: 833.41 req/s<br>
        Generic Hash	: 992.10 req/s<br>
        
        Berdasarkan data request per second tersebut, berikut adalah analisis dari setiap algoritma load balancer:
        
        Round Robin<br>
        Algoritma Round Robin mendistribusikan request secara berurutan ke setiap server yang tersedia dalam siklus tertentu. Dalam proses ini, setiap request akan dialokasikan ke server berikutnya dalam urutan yang telah ditentukan sebelumnya. Berdasarkan apa yang sudah kami jalankan, algoritma round robin menerima 495.82 request per second. Angka tersebut mengakibatkan algoritma Round Robin berkemungkinan menjadi load balancer yang paling baik dalam membagi request secara merata ke semua server.
        
        IP Hash<br>
        Algoritma IP Hash mendistribusikan request ke server berdasarkan alamat IP dari klien yang melakukan request. Dalam hal ini, algoritma ini menggunakan informasi alamat IP sebagai kunci untuk menentukan server tujuan. Pada hasil kami, Algoritma IP hash memiliki rata-rata request per second yang cukup rendah, yaitu 669.21 request per second. Hal ini menjadikan algoritma IP hash sebagai algoritma yang dapat menyelesaikan request lebih cepat dibanding dengan algoritma lainnya.
        
        Least-Connection<br>
        Algoritma Least Connection (Koneksi Terendah) mendistribusikan request ke server dengan cara memilih server yang saat itu memiliki jumlah koneksi aktif terendah. Ini berarti bahwa server dengan sedikit koneksi saat itu akan dipilih sebagai tujuan untuk request baru. Pada hasil kami, Algoritma least-connection memiliki rata-rata request per second yang paling tinggi kedua, yaitu 833.41 request per second. Hal ini menunjukkan bahwa algoritma least-connection harus menyelesaikan seluruh request tersebut dalam waktu tertentu.
        
        Generic Hash<br>
        Tujuan utama dari Generic Hash adalah untuk mendistribusikan request secara merata dan konsisten ke server-server yang ada. Pada hasil kami, Algoritma generic hash memiliki rata-rata request per second yang paling tinggi, yaitu 992.10 request per second. Hal ini mengakibatkan algoritma generic hash membutuhkan waktu yang lebih lama dibanding dengan ketiga algoritma lainnya saat membagi request secara merata ke semua server.
        
        Kesimpulan<br>
        Algoritma Round Robin merupakan pilihan terbaik dalam load balancing untuk mendistribusikan request secara merata ke semua server karena jumlah request per secondnya lebih kecil daripada ketiga algoritma lainnya. Algoritma IP Hash menjadi alternatif terbaik jika prioritasnya adalah mendistribusikan request ke server berdasarkan alamat IP dari klien yang melakukan request, menduduki posisi kedua yang efektif, diikuti oleh algoritma Least-Connection. Algoritma Generic Hash bisa digunakan untuk  mendistribusikan request secara merata dan konsisten ke server-server yang ada, Namun mungkin tidak optimal dalam memastikan pembagian request yang merata di antara semua server.

## Soal 9
>Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)

## Soal 10
>Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

Di Eisen

        apt-get install apache2-utils -y
        
        mkdir /etc/nginx/rahasisakita
        htpasswd -c /etc/nginx/rahasisakita/htpasswd netics
        
        ajkb17
        
        nano /etc/nginx/sites-available/lb-jarkom
        
        tambahkan ini
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
        
        service nginx restart

Di Revolte

        lynx granz.channel.b17.com
        
        masukkan username netics
        masukkan password b17

## Soal 11
>Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

Di Eisen

        nano /etc/nginx/sites-available/lb-jarkom
        
        tambahkan
        location ~* /its {
        proxy_pass https://www.its.ac.id;
        }
        
        service nginx restart

Di Revolte

        lynx granz.channel.b17.com/its

![Screenshot 2023-11-16 180513](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/597960f4-b1d2-4422-a264-e19160dfedb9)

## Soal 12
>Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. (12) hint: (fixed in dulu clinetnya)

Di Eisen

        nano /etc/nginx/sites-available/lb-jarkom

        tambahkan
        allow 10.17.3.69;
                allow 10.17.3.70;
                allow 10.17.4.167;
                allow 10.17.4.168;
                deny all;

Di Revolte

        lynx granz.channel.b17.com/its

![Screenshot 2023-11-16 181457](https://github.com/njabdullah/Jarkom-Modul-3-B17-2023/assets/92930757/accfa107-daf4-4b92-9c8b-78ea4d446ad6)

Terus kalau mau ngejalanin di Revolte gimana?<br>
Di Revolte

        ip a
        (lalu cek dia di IP berapa, Fixed Addresskan IP ini di Heiter)
        
Di Himmel

        nano /etc/dhcp/dhcpd.conf
        
        	host Revolte {
            hardware ethernet (eth0 ip apa);
            fixed-address 10.17.3.23;
        }
        
        service isc-dhcp-server restart

Di Revolte

        nano /etc/network/interfaces
        
        tambahkan
        auto eth0
        iface eth0 inet dhcp
        hwaddress ether (eth0 ip apa)

Di Eisen

        nano /etc/nginx/sites-available/lb-jarkom

        tambahkan IP fixed address Revolte
        allow 10.17.3.69;
                allow 10.17.3.70;
                allow 10.17.4.167;
                allow 10.17.4.168;
                allow IP fixed address Revolte;
                deny all;

Lalu lakukan test lagi di Revolte

        lynx granz.channel.b17.com/its


## Soal 13
>Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

Pada Database Server dilakukan instalasi mysql terlebih dahulu dengan script berikut

```sh
apt-get update
apt-get install mariadb-server -y
service mysql start
```
Lalu ubah konfigurasi pada `/etc/mysql/mariadb.conf.d/50-server.cnf` menjadi 

```sh
bind-address            = 0.0.0.0
```

Lalu pada `/etc/mysql/my.cnf` ubah konfigurasi menjadi

```sh

# This group is read both both by the client and the server
# use it for options that affect everything
#
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

[mysqld]
skip-networking=0
skip-bind-address
```

Lalu lakukan script berikut untuk menambahkan user dan membuat table di mysql

```sh
# Untuk login mysql terlebih dahulu
mysql -u root -p
```

```sql
# Query yang dilakukan
CREATE USER 'kelompokb17'@'%' IDENTIFIED BY 'passwordb17';
CREATE USER 'kelompokb17'@'localhost' IDENTIFIED BY 'passwordb17';
CREATE DATABASE dbkelompokb17;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokb17'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokb17'@'localhost';
FLUSH PRIVILEGES;
```

Lalu dari salah satu worker coba untuk mengakses database dengan melakukan 
```sql
mariadb --host=10.17.2.1 --port=3306 --user=kelompokb17 --password=passwordb17 dbkelompokb17 -e "SHOW DATABASES;"
```
Lalu dapat dilihat akan muncul bahwa dari worker dapat mengakses dan database yang sudah dibuat sudah ada
![img2](/img/13.1.png)
## Soal 14
>Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

Untuk menyelesaikannya pada setiap worker lakukan instalasi sebagai berikut 
```sh
apt-get update
apt-get install mariadb-client -y
apt-get install lynx -y
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

service nginx start
service php8.0-fpm start
```

Lalu setelah melakukan instalasi requirement, lakukan instalasi composer juga dengan melakukan 
```sh
cd /root
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
cp composer.phar /usr/local/bin/composer
```

Lalu apabila sudah lakukan juga instalasi untuk git dan lakukan git clone repository pada ketentuan soal dan lakukan composer update

```sh
apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update
```

Lalu lakukan juga konfigurasi untuk `.env` pada masing masing worker dengan melakukan 
```sh
cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
```

dan isikan konfigurasi berikut dengan mengisi konfigurasi database sesuai dengan ip milik kita

```php
APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.17.2.1
DB_PORT=3306
DB_DATABASE=dbkelompokb17
DB_USERNAME=kelompokb17
DB_PASSWORD=passwordb17

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"
```

Lalu pada masing masing worker konfigurasi juga untuk `nginx` nya masing masing dengan mengisi script pada `/etc/nginx/sites-available/laravel-worker`

```sh
server {
    listen <8001>;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
            try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
      include snippets/fastcgi-php.conf;
      fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
            deny all;
    }

    error_log /var/log/nginx/implementasi_error.log;
    access_log /var/log/nginx/implementasi_access.log;
}
```

sehingga dimana masing masing worker akan terbuka pada port masing masing yaitu `8001`
```sh
10.17.4.1:8001
10.17.4.2:8001
10.17.4.3:8001
```

Lalu lakukan percobaan dengan melakukan 

```sh
lynx localhost:8001
```
pada masing masing worker untuk mengetahui apakah konfigurasi sudah benar dan sudah berjalan

![img14.1](/img/14.1.png)

## Soal 15
>Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.<br>
a. POST /auth/register

Untuk melakukan testing pertama siapkan dahulu `register.json` nya yang digunakan untuk register. isikan script tersebut pada file `register.json`
```sh
cd /root && nano register.json

{
  "username": "kelompokb17",
  "password": "passwordb17"
} 
```

Lalu untuk melakukan benchmark lakukan script berikut pada client 
```sh
ab -n 100 -c 10 -p register.json -T application/json 10.17.2.2:8001/api/auth/register
```

lalu didapatkan hasil seperti berikut
![img](/img/15.1.png)

## Soal 16
>b. POST /auth/login

## Soal 17
>c. GET /me

## Soal 18
>Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)

## Soal 19
>Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan<br>
>pm.max_children<br>
pm.start_servers<br>
pm.min_spare_servers<br>
pm.max_spare_servers<br>
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)

## Soal 20
>Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)
