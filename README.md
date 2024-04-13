# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [✔️] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✔️] Commit: `Create Subscriber model struct.`
    -   [✔️] Commit: `Create Notification model struct.`
    -   [✔️] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✔️] Commit: `Implement add function in Subscriber repository.`
    -   [✔️] Commit: `Implement list_all function in Subscriber repository.`
    -   [✔️] Commit: `Implement delete function in Subscriber repository.`
    -   [✔️] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✔️] Commit: `Create Notification service struct skeleton.`
    -   [✔️] Commit: `Implement subscribe function in Notification service.`
    -   [✔️] Commit: `Implement subscribe function in Notification controller.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification service.`
    -   [✔️] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✔️] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [✔️] Commit: `Implement update method in Subscriber model to send notification HTTP requests.
    -   [✔️] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [✔️] Commit: `Implement publish function in Program service and Program controller.`
    -   [✔️] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [✔️] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Interface pada observer design pattern digunakan untuk mendefinisikan method-method atau services yang akan digunakan. Karena pada bambangshop case behavior dari observer masih belum terlalu bervariasi atau masih bisa dijadikan 1 interface subscriber, sehingga satu struktur model sudah cukup untuk observer dan behaviornya.
2. Dengan menggunakan vec (list) sebenarnya sudah cukup untuk menyimpan id yang ingin dibuat unik pada program dan url pada subscriber, tetapi vec (list) tidak memiliki mekanisme untuk memastikan keunikan dari elemennya sehingga untuk scalability ketika subscriber bertamabah banyak lebih baik menggunakan DashMap karena DashMap dapat menjamin keunikan saat melakukan pencarian atau penambahan secara efisien.
3. Untuk membuat program thread safe untuk list Subscribers (SUBSCRIBERS) dengan variabel static menggunakan DashMap dengan external library untuk thread safe HashMap adalah pilihan terbaik karena memiliki akses global ke struktur data yang sama dan dipastikan aman untuk concurrency dalam multithreaded environment. Sedangkan menggunakan Singleton Pattern (1 class hanya memiliki 1 instance) akan lebih tidak efektif dan mungkin belum perlu digunakan saat ini karena singleton pattern akan tricky untuk diimplementasikan karena adanya aturan Rust's ownership dan borrowing dan Singleton pattern memerlukan tambahan mekanisme sinkronisasi seperti mutex dan RwLock yang akan meningkatkan kompleksitas.

#### Reflection Publisher-2
1. Karena pada MVC original satu class Model memiliki banyak tanggung jawab seperti sebagai representasi data dari aplikasi, enkapsulasi penyimpanan data, dan untuk logika bisnis, di mana hal ini tidak sesuai dengan salah satu prinsip SOLID yaitu Single Responsibility Principle di mana setiap kelas harus memiliki 1 tanggung jawab sehingga service dan repository harus dipisahkan dari model, selain itu pemisahan ini juga sesuai dengan design principle untuk memisahkan part yang berbeda, memudahkan jika ada perubahan atau kesalahan, memudahkan untuk membuat testing, dan meningkatkan scalability. Setelah pemisahan model hanya memiliki tanggung jawab sebagai representasi data, service hanya untuk logika bisnis, dan repository hanya untuk operasi penyimpanan data.
2. Jika menggunakan Model tanpa ada nya pemisahan concern service dan repository, model (Program, Subscriber, Notification) akan memiliki banyak tanggung jawab seperti yang sudah disebutkan pada nomor 1 sehingga akan melanggar prinsip Single Responsibility, meningkatkan kompleksitas, code coupling, meningkatkan kemungkinan code duplicate dan redundant, mengurangi reusability, mengurangi maintainability, dan code sulit dipahami. Contoh kasus yang dapat meningkatkan kompleksitas ketika model Notification perlu mengirimkan notifikasi ke user harus berinteraksi langsung dengan model Subscriber untuk akses informasi subscriber.
3. Postman membantu untuk test program saya karena postman dapat memvalidasi respon API tanpa perlu mengakses server asli atau melibatkan pengembangan penuh dari sisi server dan tanpa membuat HTML nya, berikut fitur yang kemungkinan akan membantu saya dalam mengerjakan tugas kelompok:
- **Sending Requests**, fitur ini memungkinkan kita dengan mudah mengirim permintaan HTTP ke endpoint API dengan parameter yang dapat disesuaikan seperti header, parameter query, dan request body.
- **Automated Testing**: fitur ini memungkinkan kita membuat dan menjalankan rangkaian pengujian otomatis di Postman untuk memverifikasi perilaku endpoint API, termasuk pengujian untuk response status code, response body content, dan lainnya
- **Environment Variables**: fitur ini memungkinkan untuk mendefinisikan dan menggunakan environment variable, yang membuat mudah untuk beralih antar environment yang berbeda (misalnya, development, staging, production) saat menguji API.

#### Reflection Publisher-3
1. Pada tutorial ini menggunakan variasi Push model dari Observer Pattern. Hal ini terlihat dari deskripsi bahwa notificiation akan dikirimkan kepada subscriber yang subscribe melalui HTTP POST request yang dipicu oleh pembuatannya, promosi, atau penghapusan produk. 
2. Pull model memiliki advantage and disadvantage yaitu
+ advantage :
- Subscriber hanya mengambil data saat mereka membutuhkannya, sehingga membuat lebih efisien penggunaan sumber daya, mengurangi network traffic, dan mengurangi server load
- Pengambilan data berdasarkan kontrol dari subscriber, sehingga menghindari dari pengambilan data yang tidak perlu
- Saat banyak subscriber yang mengakses data dengan pull model server dapat mendistribusikan load lebih merata, sehingga meningkatkan scalability

+ disadvantage :
- Pada pull model subscriber harus terus request data dari server, sehingga dapat menyebabkan delay antara waktu data diperbarui di server dan saat data didapatkan oleh subscriber. Hal ini dapat membuat sistem menjadi tidak efisien ketika pembaruan data harus segera diterima.
- Implementasi Pull model mungkin memerlukan lebih banyak logika pada sisi pelanggan untuk mengelola permintaan dan pembaruan data secara efisien.
- Ada kemungkinan subscriber menerima data yang belum diperbarui karena adanya latency dan interval polling pada pull model
3. Jika tidak menggunakan multi thread saat proses notifikasi, maka proses notifikasi akan dijalankan secara sequential dan serial. Notifikasi akan diproses satu per satu, program akan menunggu sampai proses notifikasi selesai sebelum melanjutkan ke notifikasi berikutnya. Hal ini dapat menyebabkan delay saat memproses dan mengirim notifikasi ketika ada banyak notifikasi dan notifikasi membutuhkan waktu yang lama untuk dikirim, throughput notifikasi secara keseluruhan akan berkurang, saat ada banyak subscriber yang perlu dikirimkan notifikasi dapat menyebabkan bottleneck yang berdampak pada mengurangi responsiveness dan  performa aplikasi. Tidak seperti menggunakan multi thread yang dapat dapat memproses notification secara paralel, sehingga mempercepat proses dan meningkatkan responsivitas aplikasi.

