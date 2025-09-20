# Upcall

Upcall adalah mekanisme komunikasi di mana server dapat “memanggil kembali” (callback) fungsi atau memberikan respons secara asinkron setelah menerima permintaan dari client. Model ini komunikasi dua arah di mana server bisa mengirimkan notifikasi atau hasil pemrosesan tanpa harus menunggu request baru dari client.

## 1. Menjalankan Container

```
docker compose -f compose/upcall.yml up -d
```

<img src="https://imgur.com/jh3QK4g.png">

Menyalakan container untuk server dan client Upcall.

## 2. Menjalankan Server

```
docker compose -f compose/upcall.yml exec upcall-server python servercall.py
```

<img src="https://imgur.com/wHo9zwT.png">

Menjalankan server upcall yang siap menerima request dari client.

## 3. Mengecek Network Bridge

```
ip a
```

<img src="https://imgur.com/kwEzrRW.png">

Mengecek network bridge yang digunakan oleh container agar bisa ditentukan interface yang akan dipantau oleh tcpdump.

## 4. Menangkap Lalu Lintas

```
sudo tcpdump -nvi br-3d09a22d8b23 -w upcall5.pcap
```

<img src="https://imgur.com/2jIM4He.png">

Menangkap paket komunikasi antara client dan server selama proses upcall berlangsung. tcpdump mulai merekam semua paket dan menyimpannya di `upcall5.pcap`.

## 5. Menjalankan Client

```
docker compose -f compose/upcall.yml exec upcall-client python clientcall.py
```
<img src="https://imgur.com/FkjzdQz.png">

<img src="https://imgur.com/WWauxqV.png">

<img src="https://imgur.com/seqcbPS.png">

Menjalankan client yang mengirim request ke server. Client menampilkan response dari server. Jika server menggunakan mekanisme callback, client akan menerima data setelah server memprosesnya.

<img src="https://imgur.com/LN5jblW.png">

## 6. Menghentikan Container

```
docker compose -f compose/upcall.yml down
```
<img src="https://imgur.com/brZUwKe.png">

Menghentikan dan menghapus container, memastikan tidak ada proses yang tertinggal. Terminal menampilkan status `Removed` untuk client, server, dan network.
