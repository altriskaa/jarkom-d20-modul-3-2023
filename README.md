# Jarkom-Modul-3-D20-2023
Laporan Resmi Praktikum Modul 3 Kelompok D20
| Nama               |  NRP       | 
|--------------------|-------------|
| Khairuddin Nasty | 5025201041  |
| Altriska Izzati Khairunnisa Hermawan | 5025211187  |   

## Soal 0
> Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP mengarah pada worker yang memiliki IP [prefix IP].x.1.
Pada `/root/.bashrc`
```
echo '
zone "canyon.d20.com" {
        type master;
        file "/etc/bind/jarkom/canyon.d20.com";
};

zone "channel.d20.com" {
        type master;
        file "/etc/bind/jarkom/channel.d20.com";
};
' > named.conf.local

mkdir /etc/bind/jarkom
cp named.conf.local /etc/bind/
cp canyon.d20.com /etc/bind/jarkom/
cp channel.d20.com /etc/bind/jarkom/

echo '
$TTL    604800
@       IN      SOA     canyon.d20.com. root.canyon.d20.com. (
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;modu
@       IN      NS      canyon.d20.com.
@       IN      A       192.201.1.2 
riegel  IN      A       192.201.4.1
@       IN      AAAA    ::1
' > /etc/bind/jarkom/canyon.d20.com

echo '
$TTL    604800
@       IN      SOA     channel.d20.com. root.channel.d20.com. (
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      channel.d20.com.
@       IN      A       192.201.1.2
granz   IN      A       192.201.3.1
@       IN      AAAA    ::1
' > /etc/bind/jarkom/channel.d20.com

service bind9 restart
```

## Soal 1
> Lakukan konfigurasi sesuai dengan peta yang sudah diberikan
### Topologi 
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/238307c2-d8b9-463f-bfae-10fd9b5acdec)

### Initial config
#### Aura
```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.201.0.0/16

auto eth1
iface eth1 inet static
	address 192.201.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.201.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.201.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.201.4.0
	netmask 255.255.255.0
```
#### Himmel
```
auto eth0
iface eth0 inet static
	address 192.201.1.1
	netmask 255.255.255.0
	gateway 192.201.1.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Heiter
```
auto eth0
iface eth0 inet static
	address 192.201.1.2
	netmask 255.255.255.0
	gateway 192.201.1.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Denken
```
auto eth0
iface eth0 inet static
	address 192.201.2.1
	netmask 255.255.255.0
	gateway 192.201.2.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Eisen
```
auto eth0
iface eth0 inet static
	address 192.201.2.2
	netmask 255.255.255.0
	gateway 192.201.2.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Lawine
```
auto eth0
iface eth0 inet static
	address 192.201.3.1
	netmask 255.255.255.0
	gateway 192.201.3.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Linie
```
auto eth0
iface eth0 inet static
	address 192.201.3.2
	netmask 255.255.255.0
	gateway 192.201.3.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Lugner
```
auto eth0
iface eth0 inet static
	address 192.201.3.3
	netmask 255.255.255.0
	gateway 192.201.3.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Frieren
```
auto eth0
iface eth0 inet static
	address 192.201.4.1
	netmask 255.255.255.0
	gateway 192.201.4.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Flamme
```
auto eth0
iface eth0 inet static
	address 192.201.4.2
	netmask 255.255.255.0
	gateway 192.201.4.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
#### Fern
```
auto eth0
iface eth0 inet static
	address 192.201.4.3
	netmask 255.255.255.0
	gateway 192.201.4.0
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
## Soal 2
> Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
#### Aura (DHCP Relay)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y

echo '
SERVERS = “192.201.1.1”
INTERFACES = “eth1 eth2 eth3 eth4” 
' > /etc/default/isc-dhcp-relay

echo ' 
net.ipv4.ip_forward=1
service isc-dhcp-relay restart
' > /etc/sysctl.conf
```

#### Himmel (DHCP Server)
```
apt -get update
apt-get install isc-dhcp-server -y

echo ' 
INTERFACESv4="eth0"
INTERFACESv6=""
' > /etc/default/isc-dhcp-server

echo '
subnet 192.201.1.0 netmask 255.255.255.0 {
}

subnet 192.201.2.0 netmask 255.255.255.0 {
}

subnet 192.201.3.0 netmask 255.255.255.0 {
    range 192.201.3.16 192.201.3.32;
    range 192.201.3.64 192.201.3.80;
    option routers 192.201.3.0;
    option broadcast-address 192.201.3.255;
    option domain-name-servers 192.201.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.201.4.0 netmask 255.255.255.0 {
    range 192.201.4.12 192.201.4.32;
    range 192.201.4.160 192.201.4.168;
    option routers 192.201.4.0;
    option broadcast-address 192.201.4.255;
    option domain-name-servers 192.201.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}
' > /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
```

#### Heiter (DNS Server)
```
echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

      // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
```
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/082e65c3-917c-4d64-95c4-0b7e3a7673fc)

## Soal 3
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/97df33f9-761b-4e43-9a00-bd8971d68556)

## Soal 4
> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
Untuk memeriksa apakah Client telah terhubung dengan internet, dilakukan `ping google.com`
#### Revolte
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/4b9d30af-bc39-49f2-89d5-5d4e6527d487)
#### Richter
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/0a7ee362-2273-4917-9990-b85e0f5af795)
#### Sein
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/697ac612-ae56-4a1f-ad18-d015c6209757)
#### Stark
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/ce1a3db5-7cbc-414d-91c2-ede57bc26055)

## Soal 5
> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
![image](https://github.com/altriskaa/jarkom-d20-modul-3-2023/assets/114663340/082e65c3-917c-4d64-95c4-0b7e3a7673fc)

 
## Soal 6
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website [berikut](https://drive.google.com/file/d/1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1/view?usp=sharing) dengan menggunakan php 7.3.  
### PHP Worker (Lawine, Linie, Lugner)
```
apt-get update
apt-get install nginx
apt-get install php7.3-fpm
service nginx start
apt-get install wget -y
apt-get install unzip -y
apt-get install php7.3

cd /var/www
wget 'https://drive.google.com/uc?export=download&id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1'
unzip granz.channel.yyy.com.zip
rm granz.channel.yyy.com.zip
```
``` sh
echo "
server {
    listen 80;
    server_name granz.channel.d20.com;
    root /var/www/granz.channel.yyy.com/modul3;

    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}" > /etc/nginx/sites-available/granz.channel.d20.com

ln -s /etc/nginx/sites-available/granz.channel.d20.com /etc/nginx/sites-enabled

service nginx restart
```
## Soal 7
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut:  
> - Lawine, 4GB, 2vCPU, dan 80 GB SSD.
> - Linie, 2GB, 2vCPU, dan 50 GB SSD.
> - Lugner 1GB, 1vCPU, dan 25 GB SSD.

> aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. 
### Eisen
``` sh
echo "
pm = dynamic
pm.max_children = 100
pm.start_servers = 20
pm.min_spare_servers = 10
pm.max_spare_servers = 30
" > /etc/php/7.3/fpm/pool.d/www.conf

service php7.3-fpm restart
```
### Lawine
``` 
apt-get update
apt-get install apache2-utils
```
``` sh
ab -n 1000 -c 100 http://192.201.2.2/
```
## Soal 8
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:  
> - Nama Algoritma Load Balancer
> - Report hasil testing pada Apache Benchmark
> - Grafik request per second untuk masing masing algoritma. 
> - Analisis
### Eisen
#### Round Robin
``` sh
echo "
upstream backend_round_robin {
    server 192.201.3.1;
    server 192.201.3.2;
    server 192.201.3.3;
}

server {
    listen 8001;
    server_name granz.channel.d20.com;

    location / {
        proxy_pass http://backend_round_robin;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }

    error_log /var/log/nginx/lb_error_round_robin.log;
    access_log /var/log/nginx/lb_access_round_robin.log;
}" > /etc/nginx/sites-available/lb-jarkom_round_robin
```
#### Least Connection
``` sh
echo "
upstream backend_least_conn {
    least_conn;
    server 192.201.3.1;
    server 192.201.3.2;
    server 192.201.3.3;
}

server {
    listen 8002;
    server_name granz.channel.d20.com;

    location / {
        proxy_pass http://backend_least_conn;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }

    error_log /var/log/nginx/lb_error_least_conn.log;
    access_log /var/log/nginx/lb_access_least_conn.log;
}" > /etc/nginx/sites-available/lb-jarkom_least_conn
```
#### IP Hash
``` sh
echo "
upstream backend_ip_hash {
    ip_hash;
    server 192.201.3.1;
    server 192.201.3.2;
    server 192.201.3.3;
}

server {
    listen 8003;
    server_name granz.channel.d20.com;

    location / {

        proxy_pass http://backend_ip_hash;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }

    error_log /var/log/nginx/lb_error_ip_hash.log;
    access_log /var/log/nginx/lb_access_ip_hash.log;
}" > /etc/nginx/sites-available/lb-jarkom_ip_hash
```
#### Generic Hash
``` sh
echo "
upstream backend_generic_hash {
    hash $request_uri consistent;
    server 192.201.3.1;
    server 192.201.3.2;
    server 192.201.3.3;
}

server {
    listen 8004;
    server_name granz.channel.d20.com;

    location / {
        proxy_pass http://backend_generic_hash;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }

    error_log /var/log/nginx/lb_error_generic_hash.log;
    access_log /var/log/nginx/lb_access_generic_hash.log;
}" > /etc/nginx/sites-available/lb-jarkom_generic_hash
```
```
unlink /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/lb-jarkom_round_robin /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/lb-jarkom_least_conn /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/lb-jarkom_ip_hash /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/lb-jarkom_generic_hash /etc/nginx/sites-enabled/

service nginx restart
```
### Revolte
```
apt-get update
apt-get install apache2-utils

# round robin
ab -n 200 -c 10 http://granz.channel.d20.com:8001/

# least connection
ab -n 200 -c 10 http://granz.channel.d20.com:8002/

# ip hash
ab -n 200 -c 10 http://granz.channel.d20.com:8003/

# generic hash
ab -n 200 -c 10 http://granz.channel.d20.com:8004/
```
## Soal 9
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.  
### Eisen
atur jumlah worker pada baris berikut  
```
upstream backend_round_robin {
    server 192.201.3.1;
    server 192.201.3.2;
    server 192.201.3.3;
}
```
### Revolte
```
ab -n 100 -c 10 http://granz.channel.d20.com:8001/
```
## Soal 10
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/   
### Eisen
``` sh
...

server {
    ...

    location / {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;
        ...
    }

    ...
}
```
``` sh
echo "netics:ajkd20" > /etc/nginx/rahasisakita/htpasswd
```
## Soal 11
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id.  
### Eisen
```
...

server {
    ...

    location / {
        ...
    }

    location /its {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;

        proxy_pass https://www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
    }
    ...
}
```
## Soal 12
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.  
### Eisen
```
...

server {
    ...

    allow 192.201.3.69;
    allow 192.201.3.70;
    allow 192.201.4.167;
    allow 192.201.4.168;
    deny all;

    location / {
        ...
    }

    location /its {
        ...
    }
    ...
}
```
