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

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1) Dalam diagram pola Observer yang dijelaskan dalam buku *Head First Design Patterns*, *Subscriber* biasanya didefinisikan sebagai sebuah antarmuka (interface) yang menentukan bagaimana menerima pembaruan. Namun, dalam kasus *BambangShop*, *Subscriber* hanyalah sebuah *struct* yang hanya menyimpan data tanpa perilaku khusus. Karena semua *subscriber* memiliki cara kerja yang sama, maka penggunaan antarmuka atau *trait* tidak diperlukan untuk saat ini. Antarmuka hanya dibutuhkan jika terdapat berbagai jenis *subscriber* dengan perilaku yang berbeda. Oleh karena itu, cukup menggunakan satu *struct* saja.  

2) *id* dalam *Program* dan *url* dalam *Subscriber* harus bersifat unik. Jika menggunakan *Vec* (list), penyimpanan nilai unik memang memungkinkan, tetapi kurang efisien. Anda perlu melakukan pengecekan duplikasi secara manual dan mencari elemen dalam daftar, yang akan menjadi lebih lambat seiring bertambahnya data. Sementara itu, *DashMap* menawarkan akses yang lebih cepat, menjamin keunikan berdasarkan *key*, dan memudahkan penghapusan data. Oleh karena itu, *DashMap* adalah pilihan yang lebih tepat untuk kasus ini.  

3) Dalam pemrograman Rust, compiler memiliki aturan ketat untuk memastikan program aman dari *race condition* dalam lingkungan multi-*thread*. Pada variabel statis *SUBSCRIBERS*, kita menggunakan *DashMap* karena merupakan struktur data *HashMap* yang aman untuk penggunaan dalam banyak *thread*. Jika kita menerapkan pola *Singleton*, itu hanya akan memastikan ada satu instance global, tetapi tidak menjamin keamanan *thread*. Dalam kasus ini, karena beberapa *thread* dapat mengakses data secara bersamaan, *DashMap* tetap diperlukan untuk menghindari masalah *race condition* dan memastikan akses yang aman.

#### Reflection Publisher-2

#### Reflection Publisher-3