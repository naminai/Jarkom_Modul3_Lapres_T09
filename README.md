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

__(2)__ Setting __Surabaya__ agar menjadi __DHCP Relay__ antara DHCP Server dan Client! 

__(3)__ Client pada subnet __1__ mendapatkan range IP dari __192.168.0.10__ sampai __192.168.0.100__ dan __192.168.0.110__ sampai __192.168.0.200__  

__(4)__ Client pada subnet __3__ mendapatkan range IP dari __192.168.1.50__ sampai __192.168.1.70__   

__(5)__ Client mendapatkan __DNS Malang__ dan __DNS 202.46.129.2__ dari DHCP  

__(6)__ Client di subnet __1__ mendapatkan peminjaman alamat IP selama __5 menit__, sedangkan client pada subnet __3__ mendapatkan peminjaman IP selama __10 menit__ 

__(7)__ Setting user autentikasi proxy dengan format: 
- User : userta_t09 
- Password : inipassw0rdta_t09  

__(8)__ Setting agar proxy hanya dapat diakses pada hari __Selasa-Rabu__ pukul __13.00-18.00__  

__(9)__ Setting agar proxy hanya dapat diakses pada hari __Selasa-Kamis__ pukul __21.00-09.00 keesokan harinya__  

__(10)__ Setting agar ketika user ingin mengakses __google.com__ akan di redirect ke __monta.if.its.ac.id__

__(11)__ Mengubah __error page default squid__ ke page custom yang telah disediakan 

__(12)__ Setting agar ketika mengakses Proxy dapat melalui domain janganlupa-ta.t09.pw dengan port 8080
