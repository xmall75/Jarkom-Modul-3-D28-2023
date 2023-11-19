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

```Lakukan konfigurasi sesuai dengan peta yang sudah diberikan dan semua CLIENT harus menggunakan konfigurasi dari DHCP Server```

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
```
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
```
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
```
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
```
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
```
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
```

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

## No. 5
Soal :

## No. 6
Soal :

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
