# ZMQ: ZeroMQ

ZeroMQ (ZMQ) adalah library messaging yang melakukan komunikasi antar-proses atau antar-mesin dengan performa tinggi.
ZeroMQ mendukung banyak pola komunikasi, seperti:
- Request/Reply (REQ/REP) → seperti client-server biasa.
- Publish/Subscribe (PUB/SUB) → broadcast data ke banyak subscriber.
- Push/Pull (PUSH/PULL) → distribusi beban kerja ke banyak worker.

## 1. Menjalankan Container

```
docker compose -f compose/zmq.yml up -d
```

<img src="https://imgur.com/HngMMUr.png">

Menjalankan semua container yang dibutuhkan untuk percobaan ZMQ: publisher, subscriber, requester, replier, pusher, dan puller.

## 2. Mengecek Network Bridge

```
ip a
```

<img src="https://imgur.com/nvLUKsb.png">

Mengecek nama bridge yang digunakan container untuk menangkap lalu lintas menggunakan tcpdump.

## 3. Menangkap Lalu Lintas

```
sudo tcpdump -nvi br-bce966a1bc62 -w zero3.pcap
```

<img src="https://imgur.com/Q7FVZoe.png">

Status listening muncul, menandakan tcpdump siap merekam paket.

## 4. Menguji Pola Request/Reply

```
docker compose -f compose/zmq.yml exec zmq-req python clientzmq.py
```

<img src="https://imgur.com/EBDsQr7.png">

<img src="https://imgur.com/2c3jtub.png">

Mengirim request dari client (REQ) ke server (REP) dan menunggu balasan. Client menampilkan `Received reply 0: b'World'`, artinya komunikasi request-reply berhasil.

## 5. Menguji Pola Push/Pull

```
docker compose -f compose/zmq.yml exec zmq-pull python pullzmq.py
```
<img src="https://imgur.com/Vt38zj5.png">

Menjalankan pull worker yang menerima pekerjaan dari push node.
Worker menampilkan hasil pekerjaan, misalnya `Worker 1 received work: 13`.

## 6. Menguji Pola Publish/Subscribe

```
docker compose -f compose/zmq.yml exec zmq-sub python subzmq.py
```
<img src="https://imgur.com/WR12SeQ.png">

<img src="https://imgur.com/SKi1oZt.png">

Menjalankan subscriber yang menerima pesan dari publisher. Subscriber menerima pesan waktu secara realtime, seperti: Received: WAKTU Fri Sep 19 10:44:55 2025.

<img src="https://imgur.com/bHKgw1i.png">

## 7. Menghentikan Container

```
docker compose -f compose/zmq.yml down
```
<img src="https://imgur.com/hXYDBAf.png">

Menghentikan semua container dan membersihkan network.
