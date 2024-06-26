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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Jika terdapat observer yang memiliki banyak jenis yang dapat terdiri dari berbagai macam class, maka diperlukan pembuatan observer menggunakan interface/trait. Pada kasus BambangShop observernya, subscriber hanya berupa satu class, sehingga tidak diperlukan untuk membuat suatu interface/trait.

2. Menggunakan DashMap akan mempermudah karena memetakan tiap jenis produk ke tiap subscribernya. Jika menggunakan vector, maka akan diperlukan 2 vector yaitu vector untuk menyimpan url dan vector untuk menyimpan subscribernya. Hal ini lebih mempersulit.

3. Menggunakan Dashmap dibandingkan dengan Hashmap karena dashmap adalah builtin data structure yangmana merupakan hashmap yang cocok untuk multithreading. kita memerlukan dashmap karena aplikasi BambangShop menggunakan multi threading dan Map SUBSCRIBER tersebut akan diakses oleh banyak thread. Singleton berfungsi untuk memastikan agar hanya ada 1 instance dari objek tersebut selama program berjalan. Hal tersebut berfungsi agar daftar subscriber terhadap produk hanya berada pada 1 dashmap.

#### Reflection Publisher-2

1. Salah satu alasan perlu dipisahkan adalah agar menerapkan single responsibility principle. Service berfungsi untuk mengolah data yang didapatkan, sedangkan Repositoy lebih ke berurusan ke database. Pemisahan ini juga agar kode lebih termaintain.

2. Jika hanya menggunakan layer model tersebut, maka nantinya akan terjadi coupling yang tinggi pada program sehingga jika terdapat suatu perubahan, maka kita harus mengubah bagian code yang lain agar dapat berjalan.

3. Postman berguna untuk menguji aplikasi yang sudah dibuat dan mengecek response sesuai request yang kita buat. Melalui postman, dapat dicek method CRUD nya sehingga dapat melihat data yg diretrieve benar atau tidak.

#### Reflection Publisher-3

1. Yang dipakai adalah push model karena saat terjadi create, delete, atau update pada objek, maka notificcation service akan langsung memanggil method yang mengiterasi semua subscribernya untuk mendapatkan update terbaru.

2.  Jika menggunakan metode pull, maka setiap subscriber harus menentukan sendiri aakah perubahan yang terjadi relevan bagi mereka atau tidak. Keuntungannya adalah subscriber akan menjadi bebas untuk menentukan data apa yang diambil dan kapan. Kerungiannya adalah subscriber menjadi harus mengecek sendiri dan mengetahui struktur dari data sourcenya.

3. Jika tidak multi-threading, maka akan tercipta queue panjang saat notification service perlu menotify yang membuat pengiriman notifikasi ke tiap subscriber menjadi terhambat.