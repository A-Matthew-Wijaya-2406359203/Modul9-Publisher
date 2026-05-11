# Jawaban Pertanyaan Publisher

## a. How much data your publisher program will send to the message broker in one run?  
Dalam satu kali dijalankan, program publisher mengirim **5 message/event** ke message broker, karena `publish_event(...)` dipanggil 5 kali.

Kalau dihitung dari isi payload `UserCreatedEventMessage` saja (tanpa overhead protokol AMQP, header, dan metadata broker):
- `user_id` berisi 1 karakter (`"1"` s.d. `"5"`)
- `user_name` berisi 15 karakter (contoh: `"2406359203-Amir"`)

Karena format string Borsh menyimpan panjang string + isi string, perkiraan per message:
- `user_id`: 4 byte (panjang) + 1 byte (isi) = 5 byte
- `user_name`: 4 byte (panjang) + 15 byte (isi) = 19 byte
- Total per message = **24 byte**

Total 5 message = **120 byte payload** (perkiraan isi data saja). Nilai real di jaringan bisa lebih besar karena ada overhead AMQP.

## b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?
URL itu adalah connection string untuk menghubungkan aplikasi ke broker AMQP (misalnya RabbitMQ).

Artinya:
- `amqp://` = protokol yang dipakai adalah AMQP.
- `guest` pertama = username login broker.
- `guest` kedua = password dari username tersebut.
- `localhost` = broker berjalan di mesin/komputer yang sama.
- `5672` = port default AMQP.

Kenapa publisher dan subscriber pakai URL yang sama? Karena keduanya harus terhubung ke broker yang sama supaya message yang dikirim publisher bisa diterima subscriber.

## Running RabbitMQ
![Running RabbitMQ](image.png)
