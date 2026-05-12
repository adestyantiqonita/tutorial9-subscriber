# Subscriber

## Pertanyaan

### a. What is AMQP?
AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi standar yang digunakan untuk mengirim pesan antar aplikasi melalui message broker. AMQP mendefinisikan aturan bagaimana pesan dikirim, diterima, dan diproses. RabbitMQ adalah salah satu message broker yang menggunakan protokol AMQP ini.

### b. What does `guest:guest@localhost:5672` mean?
URL `amqp://guest:guest@localhost:5672` adalah alamat koneksi ke RabbitMQ dengan rincian:
- **`guest` pertama** adalah username yang digunakan untuk login ke RabbitMQ.
- **`guest` kedua** adalah password yang digunakan untuk login ke RabbitMQ.
- **`localhost`** adalah alamat host tempat RabbitMQ berjalan, dalam hal ini adalah komputer lokal kita sendiri.
- **`5672`** adalah nomor port yang digunakan RabbitMQ untuk menerima koneksi dari publisher maupun subscriber.

## Simulation Slow Subscriber

Berikut adalah tampilan grafik RabbitMQ ketika subscriber berjalan lambat:

![Slow Subscriber](images/slow-subscribe.png)

Pada grafik "Queued messages" terlihat antrian pesan yang menumpuk. Hal ini terjadi karena subscriber dibuat lambat dengan menambahkan delay 1 detik (`thread::sleep`) untuk setiap pemrosesan pesan. Sementara itu, publisher dapat terus mengirimkan event dengan cepat. Akibatnya, pesan-pesan menumpuk di queue karena subscriber tidak mampu memproses secepat publisher mengirimkan event. Total queue di komputer saya mencapai sekitar 15 pesan karena publisher dijalankan beberapa kali secara cepat, masing-masing mengirimkan 5 event, sehingga total pesan yang masuk lebih banyak daripada yang bisa diproses subscriber dalam waktu yang sama.

## Reflection and Running at least three subscribers

Berikut adalah tampilan RabbitMQ ketika 3 subscriber berjalan bersamaan:

![Three Subscribers RabbitMQ](images/three-subscribers-rabbitmq.png)
![Three Subscribers Console](images/three-subscribers-console.png)

Dengan menjalankan 3 subscriber secara bersamaan, event yang dikirim publisher terbagi-bagi secara merata antar subscriber. Hal ini menyebabkan queue berkurang jauh lebih cepat dibandingkan hanya dengan 1 subscriber. Terlihat pada grafik bahwa spike Queued messages lebih cepat turun kembali ke 0.

Jika melihat kode publisher dan subscriber, salah satu hal yang bisa diperbaiki adalah menghapus delay `thread::sleep` pada subscriber agar pemrosesan lebih cepat. Selain itu, bisa juga menambahkan error handling yang lebih baik dan logging yang lebih informatif pada kedua program.