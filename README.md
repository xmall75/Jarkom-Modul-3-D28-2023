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
