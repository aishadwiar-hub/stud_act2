# UDP One-Way

UDP (User Datagram Protocol) adalah protokol komunikasi yang connectionless (tidak memerlukan koneksi seperti TCP). Data dikirim dalam bentuk datagram dan tidak ada jaminan data akan sampai atau sampai secara berurutan. Kelebihan UDP adalah lebih cepat dan ringan, cocok untuk aplikasi real-time seperti streaming video, VoIP, atau game online.

## 1. Menjalankan Container

```
docker compose -f compose/udp.yml up -d
```

<img src="https://imgur.com/YeKHjf2.png">

Menyalakan container untuk server dan client UDP.

## 2. Menjalankan Server

```
docker compose -f compose/udp.yml exec udp-server python serverUDP.py
```

<img src="https://imgur.com/XRFxXPD.png">

Menjalankan server UDP yang akan mendengarkan pesan di port `12345`. Terminal menampilkan `UDP server up and listening on ('0.0.0.0', 12345)` menandakan server siap menerima data.

## 3. Mengecek Network Bridge

```
ip a
```

<img src="https://imgur.com/HBtBEIr.png">

Menentukan bridge network yang digunakan agar tcpdump bisa menangkap paket UDP dengan benar.

## 4. Menangkap Lalu Lintas

```
sudo tcpdump -nvi br-4d995835fb44 -w udp4.pcap
```

<img src="https://imgur.com/oDKsS0Q.png">

Menangkap paket UDP yang dikirim client ke server. tcpdump mulai mendengarkan trafik, paket yang tertangkap akan disimpan di `udp4.pcap`.

## 5. Menjalankan Client

```
docker compose -f compose/udp.yml exec udp-client python clientUDP.py
```
<img src="https://imgur.com/fFkuSy6.png">

<img src="https://imgur.com/74uUkJn.png">

<img src="https://imgur.com/drKDOZn.png">

Menjalankan client untuk mengirim pesan ke server. Client mengirim beberapa pesan ke server, seperti “Hello, UDP server!”. Server menerima dan menampilkan alamat pengirim beserta isi pesan. Sehingga komunikasi satu arah berhasil (client → server).

<img src="https://imgur.com/fOxiywA.png">

## 6. Menghentikan Container

```
docker compose -f compose/udp.yml down
```
<img src="https://imgur.com/so9fEQB.png">

Menghentikan container client, server, dan network bridge.
