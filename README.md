# Jawaban Pertanyaan AMQP

## a. Apa itu AMQP?
**AMQP (Advanced Message Queuing Protocol)** adalah protokol standar terbuka yang ada pada lapisan aplikasi yang digunakan untuk *message-oriented middleware* (perangkat lunak penengah berorientasi pesan). Protokol ini memungkinkan berbagai macam sistem, aplikasi, atau layanan untuk bisa saling berkomunikasi dan bertukar data (pesan) secara aman, meskipun mereka dibangun menggunakan bahasa pemrograman atau platform yang berbeda. Fitur utama dari AMQP meliputi antrean pesan (*queuing*), routing, *reliability*, dan keamanan (*security*). 

RabbitMQ adalah salah satu broker pesan paling populer yang menggunakan protokol AMQP.

## b. Apa arti dari `guest:guest@localhost:5672`?
text tersebut adalah format *Connection String* atau URI (Uniform Resource Identifier) yang dipakai untuk melakukan koneksi ke server broker pesan (misal RabbitMQ). Berikut rincian dari masing-masing bagian:

* **`guest` pertama:** Ini adalah **username** (nama pengguna) default yang digunakan untuk proses otentikasi/login ke server AMQP.
* **`guest` kedua:** Ini adalah **password** (kata sandi) default untuk nama pengguna tersebut.
* **`localhost:5672`:** Ini menunjukkan address tujuan dan jalur komunikasi (*host* dan *port*).
    * **`localhost`:** Menandakan bahwa server broker pesan sedang berjalan pada komputer lokal (mesin yang sama dengan aplikasi klien yang mencoba terhubung).
    * **`5672`:** Ini adalah **nomor port default** yang digunakan secara standar oleh protokol AMQP (tanpa enkripsi SSL/TLS) untuk menerima koneksi.


### Slow Subscriber
Setelah subscriber dimodifikasi agar memproses pesan lebih lambat (misalnya dengan menambahkan delay), queue pada RabbitMQ akan meningkat. Pada percobaan ini, total queue dapat bertambah karena publisher mengirim pesan lebih cepat daripada subscriber memprosesnya.

Jika pada dashboard terlihat total queue sekitar 20, artinya ada 20 pesan yang sedang menunggu untuk diproses oleh subscriber.

Hal ini terjadi karena:

- Publisher mengirim pesan secara langsung dan cepat ke message broker.
- Subscriber mengambil pesan satu per satu.
- Karena subscriber dibuat lambat, pesan yang masuk menumpuk di queue.
- RabbitMQ menyimpan semua pesan tersebut sampai subscriber selesai memprosesnya.

Jumlah queue dapat berbeda di setiap mesin. Pada kasus saya, jika simulasi berhasil dijalankan, jumlah queue akan tergantung seberapa sering publisher dijalankan saat subscriber masih sibuk memproses pesan sebelumnya. Jika publisher dijalankan 4 kali, dan setiap run mengirim 5 event, maka total queue dapat menjadi sekitar 20 pesan.