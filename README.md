# Jarkom_Modul3_Lapres_T09
Anggota Kelompok:
- Donny Kurnia Ramadhani     (05311840000004)  
- Muhammad Zulfikar Fauzi    (05311840000012)

---
## Teknis Pengerjaan
### Default memori UML adalah 64M, kecuali :
- SURABAYA : 256M
- MALANG : 160M
- MOJOKERTO : 128M
- TUBAN : 128M  
### Menghitung dan menggunakan IP sesuai dengan NID Tuntap dan NID DMZ masing-masing kelompok  
- IP Tuntap : NID_tuntap_tiap_kelompok + 1  
- IP Interface Router SURABAYA :  
-> eth0 : NID_tuntap_tiap_kelompok + 2 
-> eth3 : NID_DMZ_tiap_kelompok + 1  
-> eth1 : 192.168.0.1  
-> eth2 : 192.168.1.1  
- IP Server (SUBNET 2) :  
-> MALANG :  NID_DMZ_tiap_kelompok + 2  
-> MOJOKERTO : NID_DMZ_tiap_kelompok + 3   
-> TUBAN : NID_DMZ_tiap_kelompok + 4  

---
## Soal
__(1)__ Buatlah topologi jaringan seperti gambar berikut! 
![topologi_modul3](https://user-images.githubusercontent.com/61267430/100487809-25179280-313d-11eb-8c4f-700a8b9f1817.png) 
Pertama-tama, kita lakukan setting uml. Berikut merupakan setting pada __topologi.sh__  
![topologi sh](https://user-images.githubusercontent.com/61267430/100524110-e5f24b80-31e7-11eb-9652-0433eb48ae16.PNG) 
Kemudian kita setting network interfaces pada setiap UML, lalu kita enable IPv4 forwarding pada DHCP Server dan Relay:    
DHCP Server (Tuban)   
![settingipv4_tuban](https://user-images.githubusercontent.com/61267430/100525996-90be3600-31f7-11eb-9b13-f8f2e1939d00.PNG)   
DHCP Relay (Surabaya)   
![IPV4_Surabaya_sysctlconf](https://user-images.githubusercontent.com/61267430/100526017-d4b13b00-31f7-11eb-872c-fa994ed1e3da.png)    

__(2)__ Setting __Surabaya__ agar menjadi __DHCP Relay__ antara DHCP Server dan Client! 
Pada DHCP Relay yaitu Surabaya kita install DHCP Relay dengan `apt-get install isc-dhcp-relay`. Kemudian kita `nano /etc/default/isc-dhcp-relay`  
![settingrelaysurabaya](https://user-images.githubusercontent.com/61267430/100524220-f22ad880-31e8-11eb-881e-6e433b81b3b9.PNG)  
Penjelasan: 
__SERVERS__ : IP Address dimana DHCP Server yang ingin di-relay kan (__IP Tuban__)  
__INTERFACES__ : Network interface mana saja yang DHCP request nya akan diteruskan (__eth1 eth2 eth3__)  
__OPTIONS__ : Options tambahan yang ingin digunakan (kosong karena tidak perlu)

__(3)__ Client pada subnet __1__ mendapatkan range IP dari __192.168.0.10__ sampai __192.168.0.100__ dan __192.168.0.110__ sampai __192.168.0.200__  
Kita download DHCP server dengan `apt-get install isc-dhcp-server` kemudian kita setting INTERFACES yang digunakan oleh Tuban pada file `/etc/default/isc-dhcp-server`  
![settingipv4_tuban](https://user-images.githubusercontent.com/61267430/100524351-0a4f2780-31ea-11eb-8e8f-e5966c310a49.PNG)   
Kemudian kita edit `/etc/dhcp/dhcpd.conf` untuk menentukan range IP   
__Subnet 1__    
![settingserversubnet1_tuban](https://user-images.githubusercontent.com/61267430/100524381-497d7880-31ea-11eb-983b-0894145ce132.PNG)
__Subnet 2__ (harus dideklarasikan, agar topologi dapat dimengerti oleh server. Tetapi tidak perlu berisi konfigurasi)   
![serttingserversubnet2_tuban](https://user-images.githubusercontent.com/61267430/100524453-ee985100-31ea-11eb-9b48-d6163c2892e2.PNG)   

__(4)__ Client pada subnet __3__ mendapatkan range IP dari __192.168.1.50__ sampai __192.168.1.70__   
Kita edit setting INTERFACES yang digunakan oleh Tuban pada file `/etc/default/isc-dhcp-server`   
__Subnet 3__   
![serttingserversubnet3_tuban](https://user-images.githubusercontent.com/61267430/100524478-0e2f7980-31eb-11eb-9a0b-1ff3ef159fc6.PNG)   

__(5)__ Client mendapatkan __DNS Malang__ dan __DNS 202.46.129.2__ dari DHCP  
Kita edit di Tuban file konfigurasi `/etc/dhcp/dhcpd.conf` dan kita tambahkan `options domain-name-servers IP Malang dan 202.46.129.2`. Kita menggunakan `,` agar tidak dikira sebagai range.   
__Untuk Client Subnet 1__   
![settingserversubnet1_tuban_DNS](https://user-images.githubusercontent.com/61267430/100524584-ef7db280-31eb-11eb-9259-6bbdfd6126cd.png)    
__Untuk Client Subnet 3__   
![serttingserversubnet3_tuban_DNS](https://user-images.githubusercontent.com/61267430/100524580-ee4c8580-31eb-11eb-90be-928688501366.png)   

__(6)__ Client di subnet __1__ mendapatkan peminjaman alamat IP selama __5 menit__, sedangkan client pada subnet __3__ mendapatkan peminjaman IP selama __10 menit__ 
Kita edit di Tuban file konfigurasi `/etc/dhcp/dhcpd.conf` dan kita ubah `default-lease-time` menjadi `300` untuk client pada __Subnet 3__ dan `600` untuk client pada __Subnet 1__. (hal ini dikarenakan lease time menggunakan satuan detik maka kita harus merubah 5 menit menjadi 300 detik dan 10 menit menjadi 600 detik).     
__Untuk Client Subnet 1__   
![settingserversubnet1_tuban_DNS](https://user-images.githubusercontent.com/61267430/100524584-ef7db280-31eb-11eb-9259-6bbdfd6126cd.png)    
__Untuk Client Subnet 3__   
![serttingserversubnet3_tuban_DNS](https://user-images.githubusercontent.com/61267430/100524580-ee4c8580-31eb-11eb-90be-928688501366.png

Dibawah ini merupakan hasil testing untuk soal __DHCP (Nomor 1~6)__:    
__Hasil cat resolv.conf pada Sidoarjo (Subnet 1) dan Banyuwangi (Subnet 2)__    
![resolvconf_sidoarjobanyuwangi](https://user-images.githubusercontent.com/61267430/100525226-78e3b380-31f1-11eb-988b-ce4069f8b679.PNG)   
__Hasil ifconfig pada Sidoarjo (Subnet 1)   
![ifconfig_sidoarjo_subnet1](https://user-images.githubusercontent.com/61267430/100525229-7aad7700-31f1-11eb-9a4d-7c2c28deeba0.PNG)   
__Hasil ifconfig pada Banyuwangi (subnet 3)   
![ifconfig_banyuwangi_subnet3](https://user-images.githubusercontent.com/61267430/100525228-7a14e080-31f1-11eb-96aa-ed9b32da4316.PNG)   

__(7)__ Setting user autentikasi proxy dengan format: 
- User : userta_t09 
- Password : inipassw0rdta_t09  
Pertama-tama sebelum kita pasang autentikasi kita harus menginstall squid dan apache2-utils dengan `apt-get install squid` dan `apt-get install apache2-utils`. Kemudian kita gunakan command `htpasswd -c /etc/squid/passwd userta_t09` dan ketika diminta memasukkan password kita masukkan `inipassw0rdta_t09`. Berikut isi dari `/etc/squid/htpasswd`.    
![settingpasswdsquid_mojokerto](https://user-images.githubusercontent.com/61267430/100524791-a3cc0880-31ed-11eb-87f3-c88ab6327cc7.PNG)  

Kemudian kita setting konfigurasi squid dengan `nano /etc/squid/squid.conf`   
__Setting squid.conf__    
![settingsquidconft_mojokerto](https://user-images.githubusercontent.com/61267430/100524823-f86f8380-31ed-11eb-9cbb-5d5579a43ac6.PNG)   
__Penjelasan squid.conf__:    
`http port 8080`                              -> Menggunakan port 8080    
`visible_hostname teyvat`                     -> Nama proxy yang terlihat        
`include /etc/squid/acl.conf`                 -> Berisi deklarasi acl `MONDSTADT` dan `GOOGLE`    
`acl all src 0.0.0.0/0.0.0.0`                 -> Deklarasi acl `all`    
`auth param basic program ...`                -> Untuk autentikasi pengguna (sama dengan di modul)
` .    .    .`    
`acl USERS proxy_auth REQUIRED`               -> Deklarasi acl `USERS` untuk autentikasi pengguna proxy   
`deny_info http://monta.if.its.ac.id GOOGLE`  -> Error page ketika mengakses acl `GOOGLE` (google.com) dialihkan ke `monta.if.its.ac.id`    
`http_reply_access deny GOOGLE`               -> Deny reply dari acl `GOOGLE`    
`deny_info ERR_ACCESS_DENIED all`             -> Error page ketika mengakses di luar waktu yang ditentukan ada pada ERR_ACCESS_DENIED    
`http_access allow USERS MONDSTADT`           -> Membolehkan akses untuk acl `USERS` dan `MONDSTADT`   
`http_access deny all`                        -> Kecuali untuk yang sudah diperbolehkan diatas, maka deny semua (direpresantisakan acl `all`).    
Prompt user authentication ketika membuka browser dengan proxy:   
![userauth_test](https://user-images.githubusercontent.com/61267430/100525971-2c02db80-31f7-11eb-8ddd-405b72c09795.png)   

__(8)__ Setting agar proxy hanya dapat diakses pada hari __Selasa-Rabu__ pukul __13.00-18.00__  
Kita buat konfigurasi acl pada `/etc/squid/acl.conf` seperti berikut    
![settingaclsquid_mojokerto](https://user-images.githubusercontent.com/61267430/100524863-45535a00-31ee-11eb-83f7-66a34511eb01.PNG)   
Pada baris pertama, kita deklarasikan `acl` bernama `MONDSTADT` dengan tipe `time` untuk batasan waktu `TW 13:00-18:00` (T = Selasa dan W = Rabu).    

__(9)__ Setting agar proxy hanya dapat diakses pada hari __Selasa-Kamis__ pukul __21.00-09.00 keesokan harinya__  
Kita buat konfigurasi acl pada `/etc/squid/acl.conf` seperti berikut    
![settingaclsquid_mojokerto](https://user-images.githubusercontent.com/61267430/100524863-45535a00-31ee-11eb-83f7-66a34511eb01.PNG)   
Pada baris kedua, kita deklarasikan `acl` bernama `MONDSTADT` dengan tipe `time` untuk batasan waktu `TWH 21:00-23:59` (T = Selasa, W = Rabu, H = Kamis).  
Pada baris ketiga, kita deklarasikan `acl` bernama `MONDSTADT` dengan tipe `time` untuk batasan waktu `WHF 00:00-09:00` (W = Rabu, H = Kamis, F = Jumat).   

__(10)__ Setting agar ketika user ingin mengakses __google.com__ akan di redirect ke __monta.if.its.ac.id__   
Kita deklarasikan `acl` bernama `GOOGLE` dengan `dstdomain google.com` pada `/etc/squid/acl.conf`.    
![settingaclsquid_mojokerto](https://user-images.githubusercontent.com/61267430/100524863-45535a00-31ee-11eb-83f7-66a34511eb01.PNG)  

Lalu kita tambahkan setting berikut pada `/etc/squid/squid.conf`    
![REDIRECT](https://user-images.githubusercontent.com/61267430/100524980-8730d000-31ef-11eb-878a-bd1f1e8cde78.png)    

Hasil redirect pada jam yang telah ditentukan:   
![hasil_redirect](https://user-images.githubusercontent.com/61267430/100525950-ef36e480-31f6-11eb-9c42-3f7029f31f3d.png)    

__(11)__ Mengubah __error page default squid__ ke page custom yang telah disediakan 
Pertama-tama kita download custom error page dengan menggunakan command `wget 10.151.36.202/ERR_ACCESS_DENIED`. Lalu kita temukan bahwa default error page squid ada pada folder `/usr/share/squid/errors/English`.   
Berikut lokasi error page default squid:    
![ERR_ACCESS_DENIED](https://user-images.githubusercontent.com/61267430/100525035-decf3b80-31ef-11eb-898c-3f84d2bed511.PNG)   

Kemudian kita `cp -r` file yang telah kita download tadi pada file default page tersebut.   
Berikut merupakan error page yang muncul apabila kita mengakses di waktu yang salah:    
![errorPage_salahjam](https://user-images.githubusercontent.com/61267430/100525934-c6165400-31f6-11eb-92f7-4625565f9f3e.png)    

__(12)__ Setting agar ketika mengakses Proxy dapat melalui domain janganlupa-ta.t09.pw dengan port 8080
Untuk dapat melakukan hal diatas kita harus mengkonfigurasikan DNS pada DNS Server di Malang.   
Pertama-tama kita edit setting DNS pada `/etc/bind/named.conf.options` seperti berikut:   
![Malang named conf options](https://user-images.githubusercontent.com/61267430/100526033-fad6db00-31f7-11eb-87bf-e3b3054fe9a1.png)   

Kita setting zone pada `/etc/bind/named.local.conf` di Malang dengan konfigurasi seperti berikut:   
![Malang namedconflocal](https://user-images.githubusercontent.com/61267430/100526037-fd393500-31f7-11eb-8e5a-ddc330906284.png)   

Kemudian kita buat file konfigurasi DNS pada `/etc/bind/jarkom/janganlupa-ta.t09.pw` seperti berikut: 
![dnsjanganlupata_malang](https://user-images.githubusercontent.com/61267430/100525151-cd3a6380-31f0-11eb-9d77-be19a8f8ae2c.PNG)   

Kemudian kita dapat mengubah settingan proxy pada browser kita seperti di bawah ini untuk mengakses proxy:    
![image](https://user-images.githubusercontent.com/61267430/100525171-168ab300-31f1-11eb-81a6-504e9d6e293e.png)
