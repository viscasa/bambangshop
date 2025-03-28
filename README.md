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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

    Kebutuhan akan trait dalam Observer pattern bergantung pada kompleksitas dan variasi observer yang digunakan. Jika dalam BambangShop hanya ada satu jenis observer, yaitu `Subscriber`, maka cukup dengan satu model struct tanpa perlu trait. Namun, jika di masa depan ada kemungkinan menambahkan jenis observer lain dengan perilaku berbeda, penggunaan trait akan memberikan fleksibilitas lebih baik, memungkinkan implementasi yang lebih mudah diperluas dan sesuai dengan prinsip Open-Closed.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    Penggunaan `DashMap` lebih disarankan karena memungkinkan pemetaan langsung antara setiap jenis produk dan subscriber yang berlangganan, sehingga akses, pencarian, serta penghapusan data dapat dilakukan dengan lebih efisien. Dengan `DashMap`, kita dapat dengan mudah memastikan bahwa setiap `id` program dan `url` subscriber bersifat unik tanpa perlu iterasi manual seperti yang diperlukan dalam `Vec`. Jika menggunakan `Vec`, kita perlu struktur tambahan atau mekanisme pencarian manual untuk memastikan tidak ada duplikasi, yang dapat meningkatkan kompleksitas dan mengurangi efisiensi dalam pengelolaan data.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    Dalam konteks keamanan multithreading di Rust, penggunaan `DashMap` merupakan pilihan yang lebih tepat dibandingkan hanya menerapkan Singleton pattern. Meskipun Singleton dapat memastikan hanya ada satu instance dari objek yang dibuat, ia tidak secara otomatis menjamin thread safety. `DashMap` telah dioptimalkan untuk lingkungan multithreaded, sehingga memungkinkan operasi baca dan tulis yang lebih efisien. Oleh karena itu, dalam kasus ini, `DashMap` tetap diperlukan untuk memastikan daftar subscriber dikelola dengan aman dan optimal dalam aplikasi multithreaded.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    Memisahkan *Service* dari *Repository* dalam desain Model-View-Controller (MVC) penting untuk menerapkan prinsip *Single Responsibility* dan meningkatkan modularitas sistem. *Service* bertanggung jawab atas logika bisnis dan pengolahan data, sedangkan *Repository* berfungsi sebagai lapisan akses data yang berinteraksi langsung dengan database. Dengan pemisahan ini, perubahan dalam penyimpanan data tidak akan memengaruhi logika bisnis, begitu juga sebaliknya, sehingga kode menjadi lebih terstruktur, mudah diuji, serta lebih fleksibel untuk dikembangkan dan dipelihara.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    Jika hanya menggunakan *Model* tanpa memisahkan *Service* dan *Repository*, maka setiap model seperti *Program*, *Subscriber*, dan *Notification* harus menangani logika bisnis serta akses data secara langsung. Ini menyebabkan ketergantungan tinggi antar komponen, di mana setiap perubahan pada satu model dapat memengaruhi seluruh sistem, meningkatkan kompleksitas kode, dan menyulitkan pemeliharaan. Interaksi antar model juga menjadi lebih kusut karena setiap model harus mengetahui detail penyimpanan data dan aturan bisnis lainnya, sehingga sulit untuk melakukan perubahan atau pengujian secara terpisah tanpa merusak bagian lain dari aplikasi.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    Ya, Postman adalah alat yang sangat membantu dalam menguji dan memvalidasi fungsionalitas aplikasi dengan cara mengirimkan permintaan HTTP ke berbagai endpoint dan memeriksa respons yang diterima. Saya dapat dengan mudah melakukan pengujian CRUD (Create, Read, Update, Delete) untuk memastikan bahwa API bekerja sesuai harapan. Fitur seperti *Collection* memungkinkan saya menyimpan dan mengatur serangkaian permintaan untuk pengujian berulang, sementara *Environment Variables* membantu dalam mengelola berbagai konfigurasi API tanpa harus mengubah setiap permintaan secara manual. Selain itu, fitur *Automated Testing* dan *Mock Server* memungkinkan saya untuk mensimulasikan respons API dan menguji skenario tanpa harus bergantung pada backend yang sudah berjalan. Dengan semua fitur ini, Postman menjadi alat yang sangat bermanfaat dalam proyek di masa depan.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    Dalam tutorial ini kita menggunakan *Push Model* dari Observer Pattern. Model ini diterapkan saat aplikasi secara otomatis mengirimkan notifikasi ke para subscriber setiap kali terjadi perubahan pada produk, seperti pembuatan (*create*), penghapusan (*delete*), atau promosi (*publish*). Notifikasi ini dikirim secara langsung menggunakan HTTP POST request ke URL yang telah didaftarkan oleh subscriber, tanpa subscriber perlu meminta (*pull*) informasi secara berkala. Hal ini membuat sistem lebih responsif dan efisien karena subscriber tidak perlu melakukan polling ke server untuk mendapatkan pembaruan.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    Jika kita menggunakan metode *Pull Model*, setiap subscriber harus secara aktif memeriksa apakah ada perubahan data yang relevan bagi mereka, yang memberi mereka kendali penuh atas data yang diambil dan waktu pengambilannya. Keuntungan dari pendekatan ini adalah subscriber dapat lebih selektif dan hanya mengambil informasi yang benar-benar dibutuhkan, sehingga mengurangi beban komunikasi yang tidak perlu. Namun, kekurangannya adalah subscriber harus memiliki pemahaman yang cukup tentang struktur data sumber untuk melakukan polling secara efektif, serta ada risiko keterlambatan dalam memperoleh informasi terbaru karena data hanya diperbarui saat subscriber melakukan permintaan.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    Jika kita memutuskan untuk tidak menggunakan *multi-threading* dalam proses notifikasi, maka notifikasi akan dikirimkan secara *sekuensial*, di mana setiap subscriber harus menunggu hingga proses pengiriman ke subscriber sebelumnya selesai sebelum menerima notifikasinya. Hal ini dapat menyebabkan penundaan yang signifikan, terutama jika jumlah subscriber banyak atau jika ada latensi dalam pengiriman HTTP request. Selain itu, beban kerja akan meningkat pada satu *thread* utama, yang dapat menghambat kinerja aplikasi secara keseluruhan dan membuat sistem menjadi kurang responsif, terutama jika ada operasi lain yang berjalan bersamaan.