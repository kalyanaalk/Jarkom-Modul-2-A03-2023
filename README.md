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

## No. 2

### Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

## No. 3

### Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

## No. 4

### Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

## No. 5

### Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

## No. 6

### Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

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
