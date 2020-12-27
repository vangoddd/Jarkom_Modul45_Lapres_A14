# Jarkom_Modul45_Lapres_A14
Kelompok A14:
* _Rofita Siti Musdalifah (05111840000034)_
* _Vachri Attala Putra (0511184000043)_

#### Soal A
**Menentukan jumlah dan ukuran subnet**  
![alt text](/img/subnet2.jpg)  
**Membagi subnet dalam topologi**  
![alt text](/img/subnet1.jpg)  
**Tree**  
![alt text](/img/subnet3.jpg)  
**Menentukan Alamat IP untuk tiap subnet**  
![alt text](/img/subnet.PNG)

#### Soal B
**Topologi Dalam UML**  
**Topologi.sh**  
```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &
uml_switch -unix switch6 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.72.61 eth1=daemon,,,switch4 eth2=daemon,,,switch3 mem=64M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=64M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch4 eth1=daemon,,,switch6 eth2=daemon,,,switch5 mem=64M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch1 mem=64M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch1 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch6 mem=64M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch6 mem=64M &

# Client
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch2 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch5 mem=64M &

```

**Setting interface**  
```
[Interface]

[MADIUN]

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9

[PROBOLINGGO]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9

[GRESIK]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.2
netmask 255.255.255.0
gateway 192.168.2.1

[SIDOARJO]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.0
gateway 192.168.1.1

[MALANG]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.73.124
netmask 255.255.255.248
gateway 10.151.73.126

[MOJOKERTO]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.73.125
netmask 255.255.255.248
gateway 10.151.73.126

[BATU]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

auto eth1
iface eth1 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 10.151.73.126
netmask 255.255.255.248

[KEDIRI]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.2.1
netmask 255.255.255.0

[SURABAYA]
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.72.62
netmask 255.255.255.252
gateway 10.151.72.61

auto eth1
iface eth1 inet static
address 192.168.0.5
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.0.2
netmask 255.255.255.252
```

#### Soal C
**Routing**  
![alt text](/img/c1.PNG)  
![alt text](/img/c2.PNG)  
![alt text](/img/c3.PNG)  

#### Soal D
Memberikan IP dinamis pada subnet Sidoarjo dan Gresik

* Install dhcp server pada MOJOKERTO dan lakukan config DHCP  
```
subnet 192.168.1.0 netmask 255.255.255.0 {
        range 192.168.1.2 192.168.1.202;
        option routers 192.168.1.1;
        option broadcast-address 192.168.1.255;
        option domain-name-servers 10.151.73.124;
        default-lease-time 600;
        max-lease-time 7200;
}

subnet 192.168.2.0 netmask 255.255.255.0 {
        range 192.168.2.2 192.168.2.212;
        option routers 192.168.2.1;
        option broadcast-address 192.168.2.255;
        option domain-name-servers 10.151.73.124;
        default-lease-time 600;
        max-lease-time 7200;
}

subnet 10.151.73.120 netmask 255.255.255.248 {
        option routers 10.151.73.126;
}

```
![alt text](/img/dconfigbaru.PNG)  
![alt text](/img/dconfigbaru2.PNG)  

* Install dan atur relay pada BATU, SURABAYA, dan KEDIRI  

![alt text](/img/d3.PNG)  
![alt text](/img/d4.PNG)  
![alt text](/img/d5.PNG)  

* Atur interface pada client agar bisa mendapatkan ip dinamis

![alt text](/img/d1.PNG)  
![alt text](/img/d2.PNG)  

#### Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi
SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan
MASQUERADE.
```
iptables -t nat -A POSTROUTING -o eth0 -s 192.168.0.0/16 -j SNAT --to 10.151.72.62
```  

#### Soal 2
Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server
yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan.
```
iptables -N LOGGING
iptables -A FORWARD -d 10.151.73.120/29 -i eth0 -p tcp --dport 22 -j LOGGING
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```  
Sebelum paket di drop, paket di log terlebih dahulu melalui chain LOGGING (Soal no 7)  

#### Soal 3
Bibah meminta kalian untuk membatasi DHCP
dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari
mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```  
Sebelum paket di drop, paket di log terlebih dahulu melalui chain LOGGING (Soal no 7)  

#### Soal 4
Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin
sampai Jumat.
```
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 192.168.1.0/24 -j REJECT
```  

#### Soal 5
Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap
harinya.
```
iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 07:00 --timestop 17:00 -j REJECT
```  

#### Soal 6
Bibah ingin SURABAYA disetting sehingga setiap
request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada
PROBOLINGGO port 80 dan MADIUN port 80.
```
#probolinggo
iptables -t nat -A PREROUTING -p tcp -d 10.151.73.124 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.10:80
#madiun
iptables -t nat -A PREROUTING -p tcp -d 10.151.73.124 -j DNAT --to-destination 192.168.0.11:80
```  
#### Soal 7
Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap
UML yang memiliki aturan drop.  
Pada semua paket yang di drop, paket di masukkan ke chain "LOGGING" terlebih dahulu. Pada chain tersebut paket akan di log kedalam sistem kemudian paket baru akan di drop

Pada UML yang mengandung perintah iptables DROP yaitu pada UML **MALANG, MOJOKERTO, dan SURABAYA** dimasukkan perintah sebagai berikut:

**SURABAYA**
```
iptables -N LOGGING
iptables -A FORWARD -p tcp --dport 22 -d 10.151.79.64/29 -i eth0 -j LOGGING
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```

**MALANG dan MOJOKERTO**
```
iptables -N LOGGING
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```
