### Reflection
## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
Dalam Unary RPC, client hanya akan mengirimkan sebuah request ke server dan mendapatkan sebuah response, sehingga cocok untuk digunakan dalam skenario di mana sebuah task dapat dijalankan hanya dengan sebuah request dan klien hanya menerima sebuah response. Contoh penggunaan Unary RPC adalah melakukan fetching terhadap informasi profil user dari server.

Sedangkan dalam Server Streaming RPC, client hanya akan mengirimkan sebuah request ke server dan server akan memberikan banyak respon berupa stream of messages dan akan terus dikirimkan sampai semua data yang diperlukan terkirim. Hal ini berguna ketika server perlu untuk mengirimkan data yang banyak kepada client, tetapi client tidak perlu mengirimkan data ke dalam server. Sebagai contoh, Server Streaming diimplementasikan ketika user membutuhkan real time update dari sebuah server tertentu.

Dalam Bi-Directional Streaming RPC, klien dan server dapat mengirimkan stream of messages ke satu sama lain, sehingga dapat melakukan komunikasi secara interaktif antara klien dan server karena kedua belah pihak dapat menerima dan mengirim data. Contoh penggunaan Bi-Directional Streaming RPC adalah aplikasi chatting di mana pengguna dapat mengirimkan pesan ke satu sama lain.

## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
Terdapat adanya validasi authorization/authentication untuk setiap potongan data yang dikirimkan oleh gRPC. Hal ini berbeda dengan REST yang hanya memerlukan 1 kali validasi untuk tiap request. Selain itu, setiap potongan data yang dikirimkan perlu dienkripsi secara terpisah agar privasi data menjadi lebih terjamin. 

## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
Memastikan sinkronisasi data berjalan lancar dan juga tidak terjadinya race condition dapat menjadi sebuah rintangan dalam mengembangkan sistem bidirectional streaming. Koneksi yang lama dapat mempersulit load balancing sehingga bisa saja terdapat akumulasi koneksi yang tidak bisa terputus sehingga workload server akan menjadi lebih besar.

## 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
Kelebihan:
- Karena sifatnya yang asynchronous, maka ReceiverStream dapat meningkatkan responsiveness dan scalability dari service gRPC
- ReceiverStream dapat diintegrasikan dengan mudah dengan library gRPC sehingga Bidirectional Streaming RPC dapat diimplementasikan dengan baik
- ReceiverStream dapat diintegrasikan dengan Tokio-based asynchronous programming sehingga dapat meningkatkan ekosistem Tokio untuk membuat performa lebih baik

Kekurangan:
- ReceiverStream memerlukan resource management yang baik untuk menghindari masalah seperti resource consumption dan juga resource leaks, sehingga cleanup resources perlu diperhatikan dengan baik.

## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
Agar dapat meningkatkan maintainability dan extensibility supaya dapat melakukan perubahan teradap fitur, developer harus dapat membuat struktur codebase dengan melakukan separation of concern ke dalam modul dan komponen yang reusable. Dengan hal tersebut, developer dapat membagi fungsionalitas ke modul-modul yang berbeda sehingga dapat memudahkan maintenance.

## 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
Penggunaan Unary streaming RPC dapat diubah menjadi server streaming RPC agar pengiriman data dapat dilakukan secara lebih cepat dan overhead koneksi antara server dan client dapat dikurangi

## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
gRPC akan melakukan koneksi pemanggilan method secara otomatis karena terdapat file proto yang mana akan memanggil function dari server kepada client, sehingga konektivitas dan operasi antar platform dapat menjadi lebih mudah.

## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
Penggunaan HTTP/2 dapat membantu program ketika terdapat banyak request dan response dalam satu koneksi tanpa perlu mematikan koneksi tersebut. Kekurangannya adalah adanya overhead yang lebih tinggi dalam segi performance ataupun memory karena sifatnya yang dapat mempertahankan 1 koneksi untuk response dan request yang memiliki jumlah yang banyak.

## 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
REST API membutuhkan waktu yang lebih lama dibandingkan gRPC karena setiap pengiriman request perlu membuat koneksi baru untuk client ke server, sedangkan gRPC dapat menggunakan 1 koneksi saja sampai semua request selesai dijalankan. Dengan adanya hal tersebut, gRPC lebih baik untuk dijalankan untuk program yang membutuhkan respon secara real time karena memiliki kecepatan yang lebih baik.

## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
Pendekatan schema-based gRPC dengan Protocol Buffers dapat memastikan validasi data yang ketat dan juga serialization yang efisien, tetapi diperlukan usaha yang lebih besar dalam mendefinisikan dan memelihara skema. Sedangkan penggunaan JSON yang tidak memerlukan skema dapat meningkatkan fleksibilitas dalam representasi data, tetapi dapat menyebabkan ambiguitas, struktur data yang tidak konsisten, dan mengurangi kemudahan server dan klien dalam mengkomunikasikan data.
