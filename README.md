Kelompok  : A03

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

## No. 8

### Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

## No. 9

### Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

## No. 10

### Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

## No. 11

### Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

## No. 12

### Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

## No. 13

### Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

## No. 14

### Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

## No. 15

### Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

## No. 16

### Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

## No. 17

### Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

## No. 18

### Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

## No. 19

### Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

## No. 20

### Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.
