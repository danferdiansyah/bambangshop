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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

**Note**: Reflection saya tulis dalam bahasa indonesia karena telah menggunakan Bahasa dari modul pertama. Terima kasih.

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. **Apakah Perlu Interface dalam Observer Pattern?**  
   Dalam Observer Pattern, penggunaan interface memudahkan fleksibilitas dalam menambahkan berbagai jenis observer tanpa mengubah codebase. Namun, dalam pada tutorial *BambangShop* sejauh ini, *Subscriber* hanya memiliki dua atribut (*url* dan *name*), tanpa variasi perilaku yang kompleks. Jika belum ada rencana menambahkan berbagai jenis *Subscriber* dengan perilaku berbeda, cukup menggunakan satu struct *Subscriber* tanpa interface.

2. **Vec vs. DashMap untuk Menyimpan Subscriber**  
   *Vec* cukup jika kita hanya membutuhkan daftar tanpa *fast search* atau *deletion* berdasarkan *url*. Namun, karena *url* harus unik, penggunaan *DashMap* lebih tepat karena memungkinkan *search*, *insert*, dan *deletion* Subscriber berdasarkan *url* lebih efisien. Selain itu, *DashMap* lebih aman untuk akses paralel dibandingkan *Vec*.

3. **Apakah Perlu DashMap atau Bisa Gunakan Singleton?**  
   *DashMap* digunakan untuk memastikan akses bersamaan (*thread-safe*) ke daftar *Subscriber*. Jika kita menggunakan *Singleton* biasa, kita perlu mengunci akses setiap kali read/write, yang dapat menghambat kinerja pada environment. *DashMap* sudah cukup baik untuk mengatasi kasus ini dengan *sharding*, sehingga tetap optimal. Jadi, tetap menggunakan *DashMap* bisa menjadi pilihan yang lebih baik dibandingkan *Singleton* biasa.

#### Reflection Publisher-2
1. **Mengapa Perlu Memisahkan Service dan Repository dalam MVC?**
Pada struktur **Model-View-Controller (MVC)**, pemisahan Service dan Repository dari Model bertujuan untuk meningkatkan maintainability dan scalability dalam proses development. Model dalam MVC seharusnya tidak menangani semua aspek logika bisnis dan akses data secara langsung, karena dapat melanggar prinsip Single Responsibility Principle (SRP). Dengan adanya Service, logika bisnis dapat dipisahkan dari Model, sehingga perubahan aturan bisnis tidak berdampak langsung pada struktur data. Lebih lanjut, Repository berfungsi untuk mengelola akses ke database sehingga mempermudah penggantian teknologi penyimpanan data tanpa memengaruhi logika bisnis yang ada. 

2. **Bagaimana Jika Hanya Menggunakan Model?**
Jika hanya menggunakan Model tanpa pemisahan Service dan Repository, maka kode akan menjadi lebih kompleks dan sulit untuk dimaintain. Code juga akan jadi lebih sulit diuji dan diperbarui. Jika ada perubahan kecil misalnya format notifikasi, maka banyak bagian kode harus mengalami modifikasi.  Akibatnya, risiko terjadinya error lebih tinggi. Selain itu, *unit testing* juga jadi relatif lebih sulit. Dengan adanya Service Layer, tugas yang lebih spesifik dapat didelegasikan.

3. **Helpful Postman tools**
untuk menguji API, Postman menjadi tool yang cukup membantu. Postman biasa saya gunakan untuk menguji endpoint API, membantu debugging, serta menyimpan berbagai request dalam collections untuk memudahkan testing ulang. Selain itu, fitur automasi testing dengan pre-request dan test scripts juga berguna untuk memastikan API berfungsi sesuai harapan sebelum diintegrate ke main project. Postman juga bisa digunakan untuk simulasi autentikasi menggunakan JWT token atau API key. 

#### Reflection Publisher-3

1. **Apa observer pattern yang digunakan pada kasus tutorial ini?**
Saya menggunakan Push model sebagai Observer Pattern dalam kasus tutorial ini, karena publisher secara aktif mengirimkan data ke subscriber menggunakan `HTTP POST request`.  

2. **Apa advantage dan disadvantage kalau menggunakan model lain?**
Jika menggunakan Pull *model instead of* push model, subscriber dapat mengontrol waktu dan frekuensi pembaruan serta hanya mengambil data saat punya *resource* yang cukup untuk memprosesnya. Selain itu, beban pada publisher bisa berkurang karena tidak perlu *maintain* status koneksi dengan subscriber, dan implementasinya akan lebih sederhana. Tapi metode ini juga memiliki kelemahan, misalnya meningkatnya *network traffic* akibat request yang tidak perlu, ada latency dalam pembaruan, serta logika subscriber yang lebih kompleks.  

3. **Bagaimana jika tidak menggunakan multi-threading pada notification process?**
Jika proses notication tidak menggunakan multi-threading, program harus menunggu setiap subscriber menerima notification sebelum melanjutkan ke subscriber berikutnya. Hal ini dapat memperlambat proses notification secara keseluruhan.
