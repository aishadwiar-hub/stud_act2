# Request and Response (TCP)

Model Request/Response adalah pola komunikasi standar antara client dan server. Dimana server menunggu permintaan dari client, sementara client mengirimkan pesan (request). Server memproses dan membalas pesan (response) dan Protokol TCP digunakan karena menjamin pengiriman data secara andal dan berurutan. Model ini banyak digunakan pada aplikasi web, API, dan sistem terdistribusi untuk memastikan setiap permintaan mendapat balasan.

## 1. Menjalankan Container

```
docker compose -f compose/reqresp.yml up -d
```

<img src="https://imgur.com/Ke84Nut.png">

Menyalakan container server dan client, sesuai definisi di file `reqresp.yml`. Dengan `-d`, container berjalan di background sehingga terminal bebas digunakan untuk langkah berikutnya.

## 2. Menangkap Lalu Lintas

```
sudo tcpdump -nvi br-f06520fa69c4 -w reqresp.pcap
```

<img src="https://imgur.com/wwcsNz8.png">

Merekam semua paket data yang melewati network bridge yang digunakan oleh container. File hasil tangkapan akan disimpan sebagai `reqresp.pcap` dan dapat dianalisis menggunakan Wireshark untuk melihat alur request dan response.

## 3. Menjalankan Server

```
docker compose -f compose/reqresp.yml exec reqresp-server python server.py
```

<img src="https://imgur.com/ICMxyt3.png">

Menjalankan program server di dalam container reqresp-server, untuk mendengarkan koneksi dari client dan siap memproses permintaan.

## 4. Menjalankan Client

```
docker compose -f compose/reqresp.yml exec reqresp-client python client.py
```

<img src="https://imgur.com/YnIm3wV.png">

Menjalankan program client di dalam container reqresp-client. Client akan mengirimkan request ke server dan menampilkan response yang diterima. Akan terlihat terlihat paket TCP berisi request dari client dan balasan response dari server di Wireshark/TCP.

<img src="https://imgur.com/A88PdlE.png">

## 5. Menghentikan Container

```
docker compose -f compose/reqresp.yml down
```
<img src="https://imgur.com/LZabeQu.png">

Menghentikan semua container yang telah dijalankan dan membersihkan environment agar tidak ada proses yang tertinggal.
