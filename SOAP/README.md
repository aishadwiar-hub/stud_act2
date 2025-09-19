# SOAP (Simple Object Access Protocol)

SOAP adalah protokol komunikasi berbasis XML yang berjalan di atas HTTP. Biasanya digunakan untuk pertukaran data antara sistem yang berbeda melalui web service. Client mengirim request dalam bentuk XML, server memprosesnya dan mengembalikan response juga dalam XML. Keunggulan dari SOAP adalah formatnya standar dan mendukung banyak bahasa pemrograman, tapi relatif lebih berat dibanding REST/JSON.

## 1. Menjalankan Container

```
docker compose -f compose/soap.yml up -d
```

<img src="https://imgur.com/PpYpMJb.png">

Menyalakan container server dan client SOAP sesuai file `soap.yml`. Terlihat `soap-server` dan `soap-client` berhasil dijalankan.

## 2. Menjalankan Server

```
docker compose -f compose/soap.yml exec soap-server python server.py
```

<img src="https://imgur.com/c0EBwo3.png">

Menjalankan program server SOAP pada container `soap-server`. Server akan mendengarkan request di port `8000`. Terminal menampilkan `SOAP server listening on http://0.0.0.0:8000`, menandakan server siap menerima request.

## 3. Mengecek Jaringan

```
ip a
```

<img src="https://imgur.com/uDO5lRO.png">

Mengecek nama bridge network yang dipakai container untuk memantau lalu lintas SOAP dengan `tcpdump`.

## 4. Menangkap Lalu Lintas

```
sudo tcpdump -nvi br-ca652d60ceba -w soap2.pcap
```

<img src="https://imgur.com/ncsL1qi.png">

Menangkap paket-paket komunikasi SOAP yang melewati bridge Docker. Terminal menampilkan status `listening`, dan paket mulai direkam.

<img src="https://imgur.com/A88PdlE.png">

## 5. Menjalankan Client

```
docker compose -f compose/soap.yml exec soap-client python client.py
```
<img src="https://imgur.com/JMyK0nF.png">

Menjalankan client yang mengirim request ke server SOAP. Client menampilkan “Hasil penjumlahan dari server SOAP: 15”, artinya request sukses diproses server. Di server terlihat log HTTP GET `/wsdl` dan POST `/`, menunjukkan adanya komunikasi SOAP berbasis HTTP. Capture di Wireshark menampilkan paket HTTP dengan payload XML.

<img src="https://imgur.com/iYHYS7w.png">

<img src="https://imgur.com/Va3MyCB.png">

<img src="https://imgur.com/k79JFCI.png">

# 6. Menghentikan Container

```
docker compose -f compose/soap.yml down
```
<img src="https://imgur.com/fE4m7E6.png">

Menghentikan dan menghapus container serta network yang dibuat. Status `Removed` muncul untuk `soap-client`, `soap-server`, dan network.
