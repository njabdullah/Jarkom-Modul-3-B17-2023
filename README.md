# Jarkom-Modul-3-B17-2023

## Anggota Kelompok
| Nama                       | NRP        |
| -------------------------- | ---------- |
| Yohanes Teguh Ukur Ginting | 5025211179 |
| Abdullah Nasih Jasir       | 5025211111 |

## Topology

## Soal 1
>Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.

## Soal 2
>Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)

## Soal 3
>Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)

## Soal 4
>Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)

## Soal 5
>Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)

## Soal 6
>Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

## Soal 7
>Kepala suku dari Bredt Region memberikan resource server sebagai berikut:<br>
a. Lawine, 4GB, 2vCPU, dan 80 GB SSD.<br>
b. Linie, 2GB, 2vCPU, dan 50 GB SSD.<br>
c. Lugner 1GB, 1vCPU, dan 25 GB SSD.<br>
aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second. (7)

## Soal 8
>Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:<br>
a. Nama Algoritma Load Balancer<br>
b. Report hasil testing pada Apache Benchmark<br>
c. Grafik request per second untuk masing masing algoritma.<br> 
d. Analisis<br>

## Soal 9
>Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)

## Soal 10
>Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/ (10)

## Soal 11
>Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

## Soal 12
>Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. (12) hint: (fixed in dulu clinetnya)

## Soal 13
>Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. (13)

## Soal 14
>Frieren, Flamme, dan Fern memiliki Riegel Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer (14)

## Soal 15
>Riegel Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire.<br>
a. POST /auth/register

## Soal 16
>b. POST /auth/login

## Soal 17
>c. GET /me

## Soal 18
>Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Riegel Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern. (18)

## Soal 19
>Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan<br> 
- pm.max_children<br>
- pm.start_servers<br>
- pm.min_spare_servers<br>
- pm.max_spare_servers<br>
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.(19)

## Soal 20
>Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second. (20)
