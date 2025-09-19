# MQTT

MQTT adalah protokol komunikasi ringan berbasis publish/subscribe yang umum dipakai pada IoT. Publisher mengirim data ke broker, dan broker akan meneruskan data itu ke semua subscriber yang berlangganan topik yang sama. Protokol ini hemat bandwidth dan cocok untuk komunikasi real-time.

## 1. Menjalankan Container MQTT

```
docker compose -f compose/mqtt.yml up -d
```

<img src="https://i.imgur.com/hKT2w5n.png">

Menyiapkan lingkungan percobaan dengan menyalakan semua container sesuai file `mqtt.yml`, termasuk broker MQTT.

## 2. Menjalankan Subscriber

```
docker compose -f compose/mqtt.yml exec mqtt-sub python sub.py
```

<img src="https://imgur.com/eM4gCef.png">

Menjalankan script subscriber di dalam container `mqtt-sub`.
Subscriber ini akan tersambung ke broker dan berlangganan pada topik, sehingga siap menerima pesan dari publisher.

## 3. Menjalankan Publisher

```
docker compose -f compose/mqtt.yml exec mqtt-pub python pub.py
```

<img src="https://imgur.com/ssz63zO.png">

Menjalankan script publisher di dalam container `mqtt-pub`. Publisher akan mengirim data (contohnya suhu) ke topik yang sudah dilanggan oleh subscriber. Hasilnya akan melihat subscriber menerima pesan secara real-time.

## 4. Cek Network Bridge

```
ip a
```

<img src="https://imgur.com/JtISZtG.png">

Menampilkan daftar network interface di sistem. Kemudian mencari interface br- atau bridge network Docker yang kemudian akan dipakai untuk memantau trafik pada langkah berikutnya.

## 5. Menangkap Lalu Lintas Jaringan

```
sudo tcpdump -nvi br-ca652d60ceba -w mqtt1.pcap
```
<img src="https://imgur.com/GJeKNwO.png">

Menangkap semua paket yang melewati bridge tersebut. File hasil tangkapan disimpan sebagai test.pcap dan bisa dibuka dengan Wireshark untuk menganalisis protokol MQTT (CONNECT, SUBSCRIBE, PUBLISH, dsb).

<img src="https://imgur.com/4mfVQs5.png">

## 6. Menghentinkan Container

```
docker compose -f compose/mqtt.yml down
```

<img src="https://imgur.com/u8fLzOP.png">

Menonaktifkan semua container yang sudah dijalankan dan membersihkan resource. Hal ini penting supaya tidak ada container yang terus berjalan dan mengganggu percobaan selanjutnya.
