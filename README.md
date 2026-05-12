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