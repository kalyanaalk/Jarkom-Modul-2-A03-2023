![image](https://github.com/kalyanaalk/Jarkom-Modul-2-A03-2023-/assets/107338432/465e3da1-6f93-43e6-b7d4-ee0248f229c4)Kelompok  : A03

Nama      : Kalyana Putri Al Kanza

NRP       : 5025211137

## No. 1

### Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

Topologi untuk kelompok A03 adalah topologi 8. Berikut adalah hasil pembuatan topologi.

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163726328072245358/image.png?ex=65409f99&is=652e2a99&hm=805ef8324d949efa31da6710f60d73f54f1abe2f0249834307b52c1b805c589e&)

Masing-masing node dikonfigurasi sebagai berikut.

- Router

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.170.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.170.2.1
	netmask 255.255.255.0
 ```
  
- AbimanyuWebServer

```
auto eth0
iface eth0 inet static
	address 192.170.1.2
	netmask 255.255.255.0
	gateway 192.170.1.1
 ```
  
- PrabukusumaWebServer

```
auto eth0
iface eth0 inet static
	address 192.170.1.3
	netmask 255.255.255.0
	gateway 192.170.1.1
```

- WisanggeniWebServer

```
auto eth0
iface eth0 inet static
	address 192.170.1.4
	netmask 255.255.255.0
	gateway 192.170.1.1
```

- NakulaClient

```
auto eth0
iface eth0 inet static
	address 192.170.1.6
	netmask 255.255.255.0
	gateway 192.170.1.1
```
  
- SadewaClient

```
auto eth0
iface eth0 inet static
	address 192.170.1.5
	netmask 255.255.255.0
	gateway 192.170.1.1
```
  
- YudhistiraDNSMaster

```
auto eth0
iface eth0 inet static
	address 192.170.2.2
	netmask 255.255.255.0
	gateway 192.170.2.1
```
  
- WerkudaraDNSSlave

```
auto eth0
iface eth0 inet static
	address 192.170.2.3
	netmask 255.255.255.0
	gateway 192.170.2.1
```
  
- ArjunaLoadBalancer

```
auto eth0
iface eth0 inet static
	address 192.170.2.4
	netmask 255.255.255.0
	gateway 192.170.2.1
```

Untuk mengetes apakah node telah terkonfigurasi dengan benar, dilakukan tes kepada node client.

```
ping google.com -c 5
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163704040862003290/image.png?ex=65408ad7&is=652e15d7&hm=3e555fd52d21edc421419e87476d56e520e0e37e19139a6c2fd3b47038629c75&)

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163704205148692540/image.png?ex=65408aff&is=652e15ff&hm=ecffe5c3dca4a320e702eb323d33c4e8e300bafa8c14c747ebcce305fa1499d7&)

## No. 2

### Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Lakukan setup sebagai berikut di DNS Master.

```
echo 'nameserver 192.170.2.2
nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
echo 'zone "arjuna.A03.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.A03.com";
};' > /etc/bind/named.conf.local
mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/arjuna.A03.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.A03.com. root.arjuna.A03.com. (
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.A03.com.
@       IN      A       192.170.2.2
www     IN      CNAME   arjuna.A03.com.
' > /etc/bind/jarkom/arjuna.A03.com
service bind9 restart
```

Lakukan pengetesan di node Arjuna.

```
ping arjuna.a03.com -c 5
ping www.arjuna.a03.com -c 5
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163704507574784010/image.png?ex=65408b47&is=652e1647&hm=c28045598aba74bde28d1aa1d39f1510fa938685c7a0752e2bf4cc3ed989cfab&)

## No. 3

### Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Lakukan setup di DNS Master.

```
echo 'zone "arjuna.A03.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type master;
        file "/etc/bind/jarkom/abimanyu.A03.com";
};' > /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.A03.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.A03.com. root.abimanyu.A03.com. (
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.A03.com.
@       IN      A       192.170.2.2
www     IN      CNAME   abimanyu.A03.com.
' > /etc/bind/jarkom/abimanyu.A03.com
service bind9 restart
```

Lakukan pengetesan di node Arjuna.

```
ping arjuna.a03.com -c 5
ping www.arjuna.a03.com -c 5
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163704798806286406/image.png?ex=65408b8c&is=652e168c&hm=681bb080163c01f5fd531d0390962d4d042be222cd0308d8d6e5ba090e5afe5c&)

## No. 4

### Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Penambahan subdomain dilakukan dengan menambah setup di /etc/bind/jarkom/abimanyu.a03.com pada DNS Master.

```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.A03.com. root.abimanyu.A03.com. (
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.A03.com.
@       IN      A       192.170.2.2
www     IN      CNAME   abimanyu.A03.com.
parikesit       IN      A       192.170.1.2
' > /etc/bind/jarkom/abimanyu.A03.com
service bind9 restart
```

Tes dilakukan sebagai berikut.

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163705224486211584/image.png?ex=65408bf2&is=652e16f2&hm=a577c9d57dd62bb636f1cce52d51a50c990f53bb2d1440f0dcd5723eaaba57a8&)

## No. 5

### Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Reverse dari IP Abimanyu (192.170.1.2) adalah 2.1.170.192. Dilakukan setup sebagai berikut.

```
echo 'zone "arjuna.A03.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type master;
        file "/etc/bind/jarkom/abimanyu.A03.com";
};

zone "1.170.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/1.170.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/jarkom/1.170.192.in-addr.arpa
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.A03.com. root.abimanyu.A03.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
1.170.192.in-addr.arpa. IN      NS      abimanyu.A03.com.
2                       IN      PTR     abimanyu.A03.com.
' > /etc/bind/jarkom/1.170.192.in-addr.arpa
service bind9 restart
```

Dilakukan tes sebagai berikut.

```
host -t PTR 192.170.1.2
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163705908291960832/image.png?ex=65408c95&is=652e1795&hm=5ef8f7eda4818bb9c429a086a23b1030de7537aef27b1530d0e6be84e89c28de&)

## No. 6

### Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Untuk membuat Werkudara sebagai DNS Slave, dilakukan setup berikut di node Yudhistira.

```
echo 'zone "arjuna.A03.com" {
        type master;
        notify yes;
        also-notify { 190.170.2.3; };
        allow-transfer {192.170.2.3; };
        file "/etc/bind/jarkom/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type master;
        notify yes;
        also-notify { 190.170.2.3; };
        allow-transfer {192.170.2.3; };
        file "/etc/bind/jarkom/abimanyu.A03.com";
};

zone "2.170.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.170.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local
service bind9 restart

```

Dilakukan juga setup berikut di node Werkudara.

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
echo 'zone "arjuna.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/abimanyu.A03.com";
};
' > /etc/bind/named.conf.local
service bind9 restart
```

Untuk mengetes, jangan lupa untuk menghentikan service bind9 di Yudhistira.

```
service bind9 stop
```

Pengetesan dilakukan sebagai berikut.

```
ping abimanyu.A03.com -c 5
```

![image](https://media.discordapp.net/attachments/1163557097816981575/1163764955745628192/image.png?ex=6540c393&is=652e4e93&hm=1f62d4f1b1009b72776b18c4560a022bf6f60293aae8300a502ab19657b74113&=&width=1057&height=242)

## No. 7

### Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

Untuk subdomain yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda, dilakukan setup sebagai berikut di node Yudhistira.

```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.A03.com. root.abimanyu.A03.com.$
                        2022100601      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.A03.com.
@       IN      A       192.170.2.2
www     IN      CNAME   abimanyu.A03.com.
ns1     IN      A       192.170.2.3
baratayuda      IN      NS      ns1
parikesit       IN      A       192.170.1.2
www.parikesit   IN      CNAME   abimanyu.A03.com
' > /etc/bind/jarkom/abimanyu.A03.com

echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

echo 'zone "arjuna.A03.com" {
        type master;
        notify yes;
        also-notify { 190.170.2.3; };
        allow-transfer {192.170.2.3; };
        file "/etc/bind/jarkom/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type master;
        notify yes;
        also-notify { 190.170.2.3; };
        allow-transfer {192.170.2.3; };
        file "/etc/bind/jarkom/abimanyu.A03.com";
};

zone "2.170.192.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/2.170.192.in-addr.arpa";
};
' > /etc/bind/named.conf.local
service bind9 restart
```

Dilakukan juga setup sebagai berikut di node Werkudara.

```
echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options

echo 'zone "arjuna.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/abimanyu.A03.com";
};

zone "baratayuda.abimanyu.A03.com" {
        type master;
        file "/etc/bind/delegasi/baratayuda.abimanyu.A03.com";
};
' > /etc/bind/named.conf.local

mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.A03.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.A03.com. root.baratayuda.abimanyu.A03.com. (
                         2022100601     ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.A03.com.
@       IN      A       192.170.1.2
www     IN      CNAME   baratayuda.abimanyu.A03.com.
' > /etc/bind/delegasi/baratayuda.abimanyu.A03.com

service bind9 restart
```

Dilakukan pengetesan sebagai berikut di

```
ping baratauda.abimanyu.A03.com -c 5
ping www.baratauda.abimanyu.A03.com -c 5

```

![image](https://media.discordapp.net/attachments/1163557097816981575/1163706799799676928/image.png?ex=65408d69&is=652e1869&hm=0183e5ceebb30e0b69516432c86093d3db4cf9082d5dab8984ca7520c6cb4b2d&=&width=846&height=663)

## No. 8

### Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Untuk membuat subdomain melalui werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu, dilakukan setup sebagai berikut di node Werkudara

```
echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
};
' > /etc/bind/named.conf.options
echo 'zone "arjuna.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/arjuna.A03.com";
};

zone "abimanyu.A03.com" {
        type slave;
        masters { 192.170.2.2; };
        file "/var/lib/bind/abimanyu.A03.com";
};

zone "baratayuda.abimanyu.A03.com" {
        type master;
        file "/etc/bind/delegasi/baratayuda.abimanyu.A03.com";
};
' > /etc/bind/named.conf.local
mkdir /etc/bind/delegasi
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.A03.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.A03.com. root.baratayuda.abimanyu.A03.com. (
                         2022100601     ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.A03.com.
@       IN      A       192.170.1.2
www     IN      CNAME   baratayuda.abimanyu.A03.com.
rjp     IN      A       192.170.1.2
www.rjp IN      CNAME   rjp.baratayuda.abimanyu.A03.com.
' > /etc/bind/delegasi/baratayuda.abimanyu.A03.com

service bind9 restart
```

Pengetesan dilakukan sebagai berikut di NakulaClient

```
ping rjp.baratayuda.A03.com -c 5
ping www.rjp.baratayuda.A03.com -c 5
```

![image](https://media.discordapp.net/attachments/1163557097816981575/1163713491052548218/image.png?ex=654093a5&is=652e1ea5&hm=b07d063b4c13bfa853b79f30b44fc6a2c44ee9340e8d97f377a2f05ed8fd526e&=&width=726&height=663)

## No. 9

### Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Hal pertama yang dilakukan adalah menginstall nginx di Router.

```
apt-get update
apt-get install nginx
service nginx start
```

Install lynx, nginx, dan php di node webserver.

```
echo 'nameserver 192.170.2.2
nameserver 192.170.2.3
nameserver 192.168.122.1
' > /etc/resolv.conf
apt-get update
apt-get install lynx
apt install nginx php php-fpm -y
```

Install apache2 dan tambahkan file index.php di Werkudara.

```
apt-get install apache2
service apache2 start
apt-get install php
php -v
echo '<?php
        phpinfo();
?>
' > /var/www/html/index.php
```

Pengetesan dilakukan bersama soal 10.

## No. 10

### Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

Dilakukan setup berikut untuk masing-masing worker, dengan xxx diganti dengan nama node untuk memudahkan pengetesan dan y adalah port masing-masing node.

```
echo 'nameserver 192.170.2.2
nameserver 192.170.2.3
nameserver 192.168.122.1
' > /etc/resolv.conf
 apt-get update && apt install nginx php php-fpm -y
 mkdir /var/www/jarkom
echo '<?php
echo "Halo, Kamu berada di xxx";
?>
' > /var/www/jarkom/index.php
echo 'server {

        listen 800y;

        root /var/www/jarkom;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 }
' > /etc/nginx/sites-available/jarkom
 ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled
rm /var/www/html -rf
rm /etc/nginx/sites-available/default
service php7.0-fpm start
service php7.0-fpm restart
service nginx restart
/etc/init.d/php7.0-fpm start

```

Dilakukan juga setup sebagai berikut di ArjunaLoadBalancer.

```
echo 'nameserver 192.170.2.2
nameserver 192.170.2.3
nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install nginx
echo ' # Default menggunakan Round Robin
upstream myweb  {
        server 192.170.1.2:8002; #IP abimanyu
        server 192.170.1.3:8001; #IP prabukusuma
        server 192.170.1.4:8003; #IP wisanggeni
}

server {
        listen 80;
        server_name arjuna.A03.com.;

        location / {
        proxy_pass http://myweb;
        }
}' > /etc/nginx/sites-available/lb-jarkom

ln -s /etc/nginx/sites-available/lb-jarkom /etc/nginx/sites-enabled
rm /var/www/html -rf
rm /etc/nginx/sites-available/default
service nginx restart
nginx -t
```

Dilakukan pengetesan berikut di node client. 

```
lynx http://192.170.1.2
lynx http://192.170.1.3
lynx http://192.170.1.4
lynx http://arjuna.A03.com 
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163774445815603200/image.png?ex=6540cc69&is=652e5769&hm=8eb1bd8807f5d63c32306943c1dba8c3f6d6b7f77cae9cc61a063d76d9a67c81&)

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163774516888096818/image.png?ex=6540cc7a&is=652e577a&hm=fa72521b8396763d2ad747a24a8cb4dd4d43e3e20b9b6079387a2f3dc2ef9507&)

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163774596999286794/image.png?ex=6540cc8d&is=652e578d&hm=18895697ec988d7ec5d5496d4e4a320890e4c569c38541ca9b98a476fcf7a204&)

## No. 11 & 12

### Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

Dilakukan setup berikut di Abimanyu.

```
apt-get update
apt-get install apache2
apt-get install wget
apt-get install unzip
apt-get install libapache2-mod-php7.0

cd /var/www
wget 'https://drive.usercontent.google.com/download?id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc&export=down$unzip abimanyu -d abimanyu.A03
mv abimanyu.A03/abimanyu.yyy.com/* abimanyu.A03
rmdir abimanyu.A03/abimanyu.yyy.com
rm abimanyu

service apache2 start
cd /etc/apache2/sites-available
echo '<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.A03
        ServerName abimanyu.A03.com
        ServerAlias www.abimanyu.A03.com

        <Directory /var/www/abimanyu.A03>
                Options +Indexes
        </Directory>

        Alias /home /var/www/abimanyu.A03/index.php/home

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
' > abimanyu.A03.com.conf
a2ensite abimanyu.A03.com.conf
service apache2 restart
```

## No. 13-16

Dilakukan setup sebagai berikut di Abimanyu.

```
cd /var/www
wget 'https://drive.usercontent.google.com/download?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS&export=download&authuser=0&confirm=t&uuid=7440274a-d695-44db-8cd9-70df5bbf7c96&at=APZUnTWw6S5Rd4s_a6CHfvUKTqQG:1696947964978' -O parikesit.abimanyu

unzip parikesit.abimanyu -d parikesit.abimanyu.A03
mv parikesit.abimanyu.A03/parikesit.abimanyu.yyy.com/* parikesit.abimanyu.A03
rmdir parikesit.abimanyu.A03/parikesit.abimanyu.yyy.com
rm parikesit.abimanyu
mkdir parikesit.abimanyu.A03/secret
cd parikesit.abimanyu.A03/secret
echo '# HTML' > secret.html

service apache2 start
cd /etc/apache2/sites-available
echo '<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.A03
        ServerName parikesit.abimanyu.A03.com
        ServerAlias www.parikesit.abimanyu.A03.com

        <Directory /var/www/parikesit.abimanyu.A03/public>
                Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.A03/secret>
                Options -Indexes
        </Directory>

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        Alias /js /var/www/parikesit.abimanyu.A03/public/js

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
' > parikesit.abimanyu.A03.com.conf
a2ensite parikesit.abimanyu.A03.com.conf
service apache2 restart
```

## No. 13

### Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

Dilakukan setup ServerName dan ServerAlias.

```
ServerName parikesit.abimanyu.A03.com
ServerAlias www.parikesit.abimanyu.A03.com
```

Pengetesan dilakukan sebagai berikut.

```
lynx parikesit.abimanyu.A03.com
```

![image](https://media.discordapp.net/attachments/1163557097816981575/1163779611482656768/image.png?ex=6540d139&is=652e5c39&hm=fef6fa471c977dab4ed7c83bd20208cffa5530b11808c49512817ddb231b0759&=&width=1057&height=595)

## No. 14

### Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

Dilakukan setup berikut. Option +Indexes agar /public dapat melakukan directory listing dan Option -Indexes agar /secret tidak dapat diakses.

```
<Directory /var/www/parikesit.abimanyu.A03/public>
	Options +Indexes
</Directory>

<Directory /var/www/parikesit.abimanyu.A03/secret>
	Options -Indexes
</Directory>
```

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163779941289164820/image.png?ex=6540d187&is=652e5c87&hm=064574f1ea79f28e42c4a0b8596ada47f1ea5f3f75945a85abaa4b8e3e43b93c&)

## No. 15

### Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163780583135125544/image.png?ex=6540d221&is=652e5d21&hm=8d92a19a499ac9d40f720e119bc59488f6c3f1267fbea572df4cabd1729664ce&)

## No. 16

### Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js 

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163781583241748560/image.png?ex=6540d30f&is=652e5e0f&hm=d1eeec85f722e98a96494565906047efe70dcb806f810295a699f9cac84ce72e&)

## No. 17

### Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

![image](https://cdn.discordapp.com/attachments/1163557097816981575/1163782658057310208/image.png?ex=6540d40f&is=652e5f0f&hm=e9f7ad9829af37a3df60b1f7b1553cc15d228ca7bd6beef431f2958b7fc4e8a0&)

## No. 18

### Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.


## No. 19

### Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)


## No. 20

### Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

