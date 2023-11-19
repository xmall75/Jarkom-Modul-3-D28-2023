# Jarkom-Modul-3-D28-2023

Anggota Kelompok D28 :
1. Revanantyo Dwigantara - 5025211113
2. Charles - 5025211082

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

## No. 8
Soal :

## No. 9
Soal :

## No. 10
Soal :

## No. 11
Soal :

## No. 12
Soal :

## No. 13
Soal :

## No. 14
Soal :

## No. 15
Soal :

## No. 16
Soal :

## No. 17
Soal :

## No. 18
Soal :

## No. 19
Soal :

## No. 20
Soal :
