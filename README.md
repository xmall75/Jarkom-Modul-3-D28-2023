# Jarkom-Modul-3-D28-2023

Anggota Kelompok D28 :
1. Revanantyo Dwigantara - 5025211113
2. Charles - 5025211082

Link Grimoire : https://docs.google.com/document/d/1Qg28-pbFTuPxj9a2nc8lROl4p9VHJgYeNYM1OZ7EQe4/edit?usp=sharing

## No. 0
Soal :

`Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP, mengarah pada worker yang memiliki IP [prefix IP].x.1.`

Kita dapat melakukan register domain dengan menambahkan zone domain dan data bind pada node Heiter.

Riegel:
```
echo 'zone "riegel.canyon.d28.com" {
        type master;
        file "/etc/bind/riegel/riegel.canyon.d28.com";
};' >> /etc/bind/named.conf.local

Mkdir /etc/bind/riegel

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.d28.com. root.riegel.canyon.d28.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.d28.com.
@       IN      A       192.205.2.2 ; IP Worker Laravel
www     IN      CNAME   riegel.canyon.d28.com.' >> /etc/bind/riegel/riegel.canyon.d28.com
```
IP 192.205.2.2 akan digunakan sebagai IP Worker Laravel.

Granz:
```
echo 'zone "granz.channel.d28.com" {
        type master;
        file "/etc/bind/granz/granz.channel.d28.com";
};' >> /etc/bind/named.conf.local

Mkdir /etc/bind/granz

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.d28.com. root.granz.channel.d28.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.d28.com.
@       IN      A       192.205.3.1 ; IP Worker PHP
www     IN      CNAME   granz.channel.d28.com.' >> /etc/bind/granz/granz.channel.d28.com
```
IP 192.205.3.2 akan digunakan sebagai IP Worker PHP.

Setelah itu, kita harus melakukan forwarding IP NAT `192.168.122.1` agar yang lain bisa mengakses internet.
```
echo 'options {
	directory "/var/cache/bind";
	forwarders {
    		192.168.122.1;
	};
	allow-query{any;};
	
	auth-nxdomain no;
	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

## No. 1
Soal :

`Lakukan konfigurasi sesuai dengan peta yang sudah diberikan dan semua CLIENT harus menggunakan konfigurasi dari DHCP Server`

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/6a4b15c6-ee8d-4eea-855c-c0979c9edfd2)

Berikut konfigurasi untuk setiap node:
- Aura
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.205.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.205.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.205.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.205.4.0
	netmask 255.255.255.0
```

- Himmel
```
auto eth0
iface eth0 inet static
	address 192.205.1.1
	netmask 255.255.255.0
	gateway 192.205.1.0
```

- Heiter
```
auto eth0
iface eth0 inet static
	address 192.205.1.2
	netmask 255.255.255.0
	gateway 192.205.1.0
```

- Denken
```
auto eth0
iface eth0 inet static
	address 192.205.2.1
	netmask 255.255.255.0
	gateway 192.205.2.0
```

- Eisen
```
auto eth0
iface eth0 inet static
	address 192.205.2.2
	netmask 255.255.255.0
	gateway 192.205.2.0
```

- Revolte
```
auto eth0
iface eth0 inet dhcp
```

- Richter
```
auto eth0
iface eth0 inet dhcp
```

- Lawine
```
auto eth0
iface eth0 inet static
	address 192.205.3.1
	netmask 255.255.255.0
	gateway 192.205.3.0
```

- Linie
```
auto eth0
iface eth0 inet static
	address 192.205.3.2
	netmask 255.255.255.0
	gateway 192.205.3.0
```

- Lugner
```
auto eth0
iface eth0 inet static
	address 192.205.3.3
	netmask 255.255.255.0
	gateway 192.205.3.0
```

- Sein
```
auto eth0
iface eth0 inet dhcp
```

- Stark
```
auto eth0
iface eth0 inet dhcp
```

- Frieren
```
auto eth0
iface eth0 inet static
	address 192.205.4.1
	netmask 255.255.255.0
	gateway 192.205.4.0
```

- Flamme
```
auto eth0
iface eth0 inet static
	address 192.205.4.2
	netmask 255.255.255.0
	gateway 192.205.4.0
```

- Fern
```
auto eth0
iface eth0 inet static
	address 192.205.4.3
	netmask 255.255.255.0
	gateway 192.205.4.0
```

## No. 2
Soal :

`Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80`

Kita bisa buat script pada node Himmel dengan isi konfigurasi sebagai berikut:
```
apt-get update -y
apt-get install isc-dhcp-server -y

echo ’
INTERFACES="eth0"
‘ >> /etc/default/isc-dhcp-server

subnet 192.205.3.0 netmask 255.255.255.0 {
    range 192.205.3.16 192.205.3.32;
    range 192.205.3.64 192.205.3.80;
    option routers 192.205.3.1;
    option broadcast-address 192.205.3.255;
    option domain-name-servers 192.205.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}
```

## No. 3
Soal :

`Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168`

Setelah itu kita atur untuk Client yang melalui Switch4.
```
subnet 192.205.4.0 netmask 255.255.255.0 {
    range 192.205.4.12 192.205.4.20;
    range 192.205.4.160 192.205.4.168;
    option routers 192.205.4.1;
    option broadcast-address 192.205.4.255;
    option domain-name-servers 192.205.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
‘ >> /etc/dhcp/dhcpd.conf
```

## No. 4
Soal :

`Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut`

Untuk melakukan ini, kita bisa mengatur `domain-name-servers` pada Himmel.
```
option domain-name-servers 192.205.1.2;
```
dan tambahkan forwarder pada Heiter.
```
echo 'options {
	directory "/var/cache/bind";
	forwarders {
    		192.168.122.1;
	};
	allow-query{any;};
	
	auth-nxdomain no;
	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```

Testing pada Client Revolte:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/04c3780e-483f-42b8-8195-0e338fbf0ce1)

Testing internet pada Client Revolte:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/e3b9cc38-d2cd-4e43-b756-1799f367a4ef)

## No. 5
Soal :

`Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit.`

Untuk melakukan hal ini, kita bisa menambahkan `default-lease-time` yaitu waktu default dan `max-lease-time` yaitu waktu maksimal pada node Himmel. Untuk Switch3 kita atur sebagai berikut:
```
subnet 192.205.3.0 netmask 255.255.255.0 {
    range 192.205.3.16 192.205.3.32;
    range 192.205.3.64 192.205.3.80;
    option routers 192.205.3.1;
    option broadcast-address 192.205.3.255;
    option domain-name-servers 192.205.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}
```
Dengan `default-lease-time` 180 secs yaitu 3 menit dan `max-lease-time` yaitu 5760 secs yaitu 96 menit.

Untuk Switch4 kita atur sebagai berikut:
```
subnet 192.205.4.0 netmask 255.255.255.0 {
    range 192.205.4.12 192.205.4.20;
    range 192.205.4.160 192.205.4.168;
    option routers 192.205.4.1;
    option broadcast-address 192.205.4.255;
    option domain-name-servers 192.205.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
```
Dengan `default-lease-time` 720 secs yaitu 12 menit dan `max-lease-time' yaitu 5760 secs yaitu 96 menit.


## No. 6
Soal :

`Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.`

Untuk melakukan deployment pertama-tama kita bisa mengunduh hal-hal yg diperlukan oleh worker seperti file web dan php7.3.
```
DEBIAN_FRONTEND=noninteractive apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y nginx php7.3 php-fpm

apt-get install wget
apt-get install unzip
apt-get install htop -y

wget --no-check-certificate "https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download" -O granz.zip
mkdir /var/www
mkdir /var/www/granz.channel.d28.com

unzip -j granz.zip -d /var/www/granz.channel.d28.com
```
Setelah persiapan selesai, kita bisa melakukan deployment dengan memasukkan konfigurasi berikut ke dalam masing-masing php worker:
```
echo 'server {

 	listen 8001;

 	root /var/www/granz.channel.d28.com;

 	index index.php index.html index.htm;
 	server_name _;

 	location / {
 			try_files $uri $uri/ /index.php?$query_string;
 	}

 	# pass PHP scripts to FastCGI server
 	location ~ \.php$ {
 	include snippets/fastcgi-php.conf;
 	fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
 	}

 location ~ /\.ht {
 			deny all;
 	}

 	error_log /var/log/nginx/granz.channel.d28.com_error.log;
 	access_log /var/log/nginx/granz.channel.d28.com_access.log;
 }' > /etc/nginx/sites-available/granz.channel.d28.com

 ln -s /etc/nginx/sites-available/granz.channel.d28.com /etc/nginx/sites-enabled

rm -rf /etc/nginx/sites-enabled/default
service php7.3-fpm restart
service php7.3-fpm start
service php7.3-fpm restart
service nginx restart
```

## No. 7
Soal :

`Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.`

Untuk melakukan itu, kita bisa mengatur pada Eisen (LB) dengan algoritma Weighted Round-Robin dan lakukan testing dengan 1000 requests dan 100 request/seconds.
```
ab -n 1000 -c 100 http://www.granz.channel.d28.com/
```
Lalu didapatkan hasil sebagai berikut:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/52de657f-b4cb-461b-ac8d-161c1360ef2d)


## No. 8
Soal :

```
Buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
a. Nama Algoritma Load Balancer.
b. Report hasil testing pada Apache Benchmark.
c. Grafik request per second untuk masing masing algoritma. 
d. Analisis.
```

a. Nama algoritma dan b. Report hasil testing.
- Round-robin

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/19c2c9f9-33c8-49e2-bd39-cfb74896b2ce)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/a3eca8a5-ec1e-4d3a-950f-0f564dfa188b)

- Weighted Round-robin

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/08ea6140-4888-4e1e-9fc5-3f357967bd34)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/f0787bfc-528c-4f45-8ed4-ed9e7a3eacba)

- Least Connection

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/180a6eec-fb71-4390-b929-76e9df7e6e6f)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/6dc2fbc9-6f69-4675-9493-888f3c495aef)

- IP Hash

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/6aacc90b-bdac-4014-837f-f9a3aeab10fc)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/c017ced1-e82c-49bd-8bb3-7abf5705628c)

- Generic Hash

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/06f0739c-6cb7-4a89-bbcb-bf5165c3b2bc)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/d46bbdeb-e6af-43e1-ac93-d14ec1dbd394)

c. Grafik Request per seconds.

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/1fd0ad10-a75c-4789-a501-95c17acf5ef9)

d. Analisis.

Dari grafik bisa dilihat algoritma yang paling bagus adalah algoritma IP Hash. 
Proses algoritma IP Hash dimulai ketika paket data tiba di jaringan. Algoritma hashing mengekstrak alamat IP sumber dan tujuan dari paket dan menggunakannya sebagai masukan. Ini kemudian menghasilkan nilai hash unik berdasarkan alamat IP ini. Nilai hash ini berfungsi sebagai pengenal kunci yang menentukan server mana dalam sekelompok server yang akan menangani paket tersebut. Keunggulan IP Hash terletak pada kemampuannya mendistribusikan beban jaringan secara merata secara konsisten. Dengan menetapkan setiap paket ke server berdasarkan nilai hash, hal ini memastikan bahwa tidak ada satu server pun yang kewalahan dengan lalu lintas. Hal ini sangat bermanfaat dalam lingkungan jaringan dengan lalu lintas tinggi di mana menjaga keseimbangan dan mencegah kelebihan beban server sangatlah penting.

## No. 9
Soal :

`
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.
`

Testing dengan 3 worker:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/ed374ec7-9056-40fd-8399-32f82dde094c)

Testing dengan 2 worker:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/52862c83-5226-4675-bb9c-c7ecf7582de1)

Testing dengan 1 worker:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/1c52befd-cc13-4fb4-afea-952425330186)

Grafik testing:

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/ab28bac2-e146-4216-87ed-c0f5039a9728)

## No. 10
Soal :

`Tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/`

Untuk bisa melakukan ini kita perlu melakukan sedikit konfigurasi pada Eisen. 

```
mkdir /etc/nginx/rahasiakita

htpasswd -b -c /etc/nginx/rahasiakita/.htpasswd netics ajkd28

echo ' 
# Default menggunakan Round Robin
 upstream myweb  {
 	server 192.205.3.1:8001; 
 	server 192.205.3.2:8002; 
	server 192.205.3.3:8003; 
 }

 server {
 	listen 80;
 	server_name granz.channel.d28.com;

 	location / {
            proxy_pass http://myweb;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;

            auth_basic "Administrators Area";
            auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }

        location ~ /\.ht {
            deny all;
        }

 }' > /etc/nginx/sites-available/lb-granz
```
Kita bisa menggunakan htpasswd untuk setting username menjadi netics dan password menjadi ajkd28. Setelah itu bisa mengikuti modul dan set nama servernya menjadi granz.channel.d28.com.

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/34641833/967252d7-67e3-4468-bf0f-86ed5893876c)


## No. 11
Soal :

`Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id`

Kita perlu menambahkan proxy untuk its di file lb-granz. Bisa menggunakan script dibawah ini.

```
echo ' # Default menggunakan Round Robin
 upstream myweb  {
 	server 192.205.3.1:8001; 
 	server 192.205.3.2:8002; 
	server 192.205.3.3:8003; 
 }

 server {
 	listen 80;
 	server_name granz.channel.d28.com;

 	location / {
            proxy_pass http://myweb;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header    Host $http_host;

            auth_basic "Administrator's Area";
            auth_basic_user_file /etc/nginx/rahasiakita/.htpasswd;
        }

        location ~ /\.ht {
            deny all;
        }
        
        location /its {
             proxy_pass https://www.its.ac.id;
        }

 }' > /etc/nginx/sites-available/lb-granz
```
Hasil lynx :

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/b22c1a24-ac49-4c57-b1d9-6fb306852dd1)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/9bd063b7-6406-4da2-8aae-44add846be41)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/d96b2dea-1633-40b8-95fc-1bdcd15c3df5)


## No. 12
Soal :

`Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.`

Kita perlu tambah ini di load balancer.

```
    allow  192.205.3.69;
    allow  192.205.3.70;
    allow  192.205.4.167;
    allow  192.205.4.168;
    deny   all;
```

Kalau misalnya kita coba akses menggunakan ip yang tidak di tentukan : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/bee1ef12-c931-4517-8543-ff7df5ab45db)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/e9efb103-6a61-4944-9a72-2a347dbdc70d)

Menggunakan ip yang boleh (bisa setting ip fixed di configuration client): 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/973f0089-7e3a-45ab-b692-15a2b46dc6b2)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/deaf477f-b5c3-4f4c-84a1-21549dfda1f0)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/59db4d08-85d9-4e47-bfcc-dbbcf532d49d)

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/b337050c-4d8b-447e-a6b4-b2455b50ecf6)


## No. 13
Soal :

`Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.`

Kita hanya perlu mengikuti modul saja untuk yang satu ini. Pertama download mariadb-server dulu.

```
apt-get update -y
apt-get install mariadb-server -y
service mysql start
```

Isi konfigurasi
```
CREATE USER 'kelompokd28'@'%' IDENTIFIED BY 'passwordd28';
CREATE USER 'kelompokd28'@'localhost' IDENTIFIED BY 'passwordd28';
CREATE DATABASE dbkelompokd28;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokd28'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokd28'@'localhost';
FLUSH PRIVILEGES;
```

Tambah ini ke /etc/mysql/my.cnf. Kemudian restart database.

```
echo ‘[mysqld]
skip-networking=0
skip-bind-address’  >> /etc/mysql/my.cnf
service mysql restart
```

Untuk di client perlu kita download `apt-get install mariadb-client -y`. Kemudian bisa kita test seperti dibawah untuk melihat apakah database yang kita buat sudah ada.

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/68b0307d-7feb-4ef4-9b33-e5f6d75de499)




## No. 14
Soal :

`Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer `

Untuk yang satu ini bisa kita ikutin saja modul implementasi dengan beberapa modifikasi agar bisa berjalan worker laravelnya.

```
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'

apt-get update -y
apt-get install php8.1-mbstring php8.1-xml php8.1-cli php8.1-common php8.1-intl php8.1-opcache php8.1-readline php8.1-mysql php8.1-fpm php8.1-curl unzip wget -y

apt-get install nginx -y

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

apt-get install git -y

cd /var/www

git clone https://github.com/martuafernando/laravel-praktikum-jarkom.git

cd laravel-praktikum-jarkom

composer install

cp .env.example .env
nano

echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=192.205.2.1
DB_PORT=3306
DB_DATABASE=dbkelompokd28
DB_USERNAME=kelompokd28
DB_PASSWORD=passwordd28

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
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > .env


cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret php artisan migrate:fresh
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret php artisan db:seed --class=AiringsTableSeeder
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret php artisan jwt:secret
```

Jika dilihat pada saya menggunakan php 8.1 bukan php 8.0 dikarenakan terdapat error yang saya temui saat melakukan composer install menggunakan php 8.0. Error tersebut mengatakan bahwa versi php yang diperlukan adalah 8.1/8.2 sehingga agar bisa jalan saya download php 8.1. Kemudian hal yang beda dari modul yakni `cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret php artisan jwt:secret`. Command ini dijalankan karena saya menemukan error yang mengatakan key secret tidak ditemukan. Sehingga kita perlu menjalankan command tersebut agar semuanya jalan. Untuk kode sisanya pada dasarnya sama dengan modul.

Hasil lynx :

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/39107333-0dc5-4261-9214-f9a91d5573d8)


## No. 15
Soal :

`Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second`

Untuk yang no 15 kita test `POST /auth/register`.

```
echo '
{
    "username": "kelompokd28",
    "password": "passwordd28"
}' > register.json

curl -X POST http://192.205.4.1:8004/api/auth/register -H 'Content-Type: application/json' -T register.json

ab -n 100 -c 10 -p register.json -T application/json http://192.205.4.1:8004/api/auth/register
```

Command yang dijalankan di client adalah yang seperti di atas. Bisa dilihat saya membuat register json dulu kemudian melakukan curl ke worker tersebut. Untuk testing kita bisa menggunakan `ab` seperti biasa.

Hasil Response (Karena saya sudah register, direturn bahwa username sudah exist):

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/318da368-7da5-41e2-9578-0094a8a429ec)

Hasil Benchmark : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/03335337-d6f7-46b6-a468-455c10fd27ff)


## No. 16
Soal :

Untuk yang no 16 sama seperti 15 tapi kita test `POST /auth/login`.

Bisa kita gunakan command di bawah ini untuk memenuhi permintaan soal.

```
echo '
{
    "username": "kelompokd28",
    "password": "passwordd28"
}' > login.json

curl -X POST http://192.205.4.1:8004/api/auth/login -H 'Content-Type: application/json' -T login.json

ab -n 100 -c 10 -p login.json -T application/json http://192.205.4.1:8004/api/auth/login
```

Hasil Response : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/e6e53dad-ccbb-411c-8d47-3b406a6e4ca4)

Hasil Benchmark : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/1a07e23d-a969-48ba-991b-3a1c3f25d2c3)


## No. 17
Soal :

Untuk yang no 17 sama seperti 2 soal di atas tapi kita test `GET /me`.

Bisa kita gunakan command di bawah ini untuk memenuhi permintaan soal.

```
# Simpan token
curl -X POST -H "Content-Type: application/json" -d @login.json http://192.205.4.1:8004/api/auth/login > login.txt

# Set token agar bisa di akses
token=$(cat login.txt | jq -r '.token')

# Test response ke api/me
curl -H "Authorization: Bearer $token" http://192.205.4.1:8004/api/me

# Benchmark test
ab -n 100 -c 10 -H "Authorization: Bearer $token" http://192.205.4.1:8004/api/me
```

Hasil Response : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/63a9f39d-fc79-4d9b-a971-f8acc3d26d46)


Hasil Benchmark : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/f8f15ee9-341d-4545-96e2-742d0948541e)


## No. 18
Soal :

`Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.`

Pada dasarnya kita diminta untuk membuat load balancer untuk ketiga worker laravel.

Jadi di eisen bisa kita jalankan command ini.

```
echo 'upstream laravel {
        server 192.205.4.1:8004;
        server 192.205.4.2:8005;
        server 192.205.4.3:8006;
}

server {
        listen 80;
        server_name riegel.canyon.d28.com;

        location / {
                proxy_pass http://laravel;
        }
}' > /etc/nginx/sites-available/lb-riegel
```

## No. 19
Soal :

```
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.
```

Testing 1 :

```
echo '
[www]
user = www-data
group = www-data
listen = /var/run/php/php8.1-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s' > /etc/php/8.1/fpm/pool.d/www.conf
```

Hasil Testing 1 : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/ca69cfdf-9997-4fc8-b4c9-12f95a81a626)


Testing 2 :

```
echo '
[www]
user = www-data
group = www-data
listen = /var/run/php/php8.1-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 100
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s' > /etc/php/8.1/fpm/pool.d/www.conf
```

Hasil Testing 2 : 

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/7bb70d38-f58f-4e03-91ce-fc8bf847a72e)


Testing 3 : 

```
echo '
[www]
user = www-data
group = www-data
listen = /var/run/php/php8.1-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 50
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.process_idle_timeout = 10s' > /etc/php/8.1/fpm/pool.d/www.conf
```

Hasil Testing 3 :

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/46619e3d-86ab-4231-ba2c-edcb20f5112a)

Jadi bisa kita lihat menggunakan setingan ke 3 bisa mendapatkan failed request dan request per second terendah.

## No. 20
Soal :

`Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.`

Kita cuma perlu mengubah settingan di load balancer agar menjadi least connection. Kita bisa mengikuti modul untuk mencapai hal tersebut.

```
echo 'upstream laravel {
        least_conn;
        server 192.205.4.1:8004;
        server 192.205.4.2:8005;
        server 192.205.4.3:8006;
}

server {
        listen 80;
        server_name riegel.canyon.d28.com;

         location / {
                proxy_pass http://laravel;
                proxy_set_header    X-Real-IP $remote_addr;
                proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header    Host $http_host;
        }
}' > /etc/nginx/sites-available/lb-riegel
```

Hasil Benchmark :

![image](https://github.com/xmall75/Jarkom-Modul-3-D28-2023/assets/115076652/b31e3ac2-4075-4a8e-a6db-bf5130ece79e)


