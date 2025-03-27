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
1) In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
 
Jawaban:
Dalam diagram pola Observer yang dijelaskan dalam buku *Head First Design Patterns*, *Subscriber* biasanya didefinisikan sebagai sebuah antarmuka (interface) yang menentukan bagaimana menerima pembaruan. Namun, dalam kasus *BambangShop*, *Subscriber* hanyalah sebuah *struct* yang hanya menyimpan data tanpa perilaku khusus. Karena semua *subscriber* memiliki cara kerja yang sama, maka penggunaan antarmuka atau *trait* tidak diperlukan untuk saat ini. Antarmuka hanya dibutuhkan jika terdapat berbagai jenis *subscriber* dengan perilaku yang berbeda. Oleh karena itu, cukup menggunakan satu *struct* saja.  

2) id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Jawaban:
*id* dalam *Program* dan *url* dalam *Subscriber* harus bersifat unik. Jika menggunakan *Vec* (list), penyimpanan nilai unik memang memungkinkan, tetapi kurang efisien. Anda perlu melakukan pengecekan duplikasi secara manual dan mencari elemen dalam daftar, yang akan menjadi lebih lambat seiring bertambahnya data. Sementara itu, *DashMap* menawarkan akses yang lebih cepat, menjamin keunikan berdasarkan *key*, dan memudahkan penghapusan data. Oleh karena itu, *DashMap* adalah pilihan yang lebih tepat untuk kasus ini.  

3) When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Jawaban:
Dalam pemrograman Rust, compiler memiliki aturan ketat untuk memastikan program aman dari *race condition* dalam lingkungan multi-*thread*. Pada variabel statis *SUBSCRIBERS*, kita menggunakan *DashMap* karena merupakan struktur data *HashMap* yang aman untuk penggunaan dalam banyak *thread*. Jika kita menerapkan pola *Singleton*, itu hanya akan memastikan ada satu instance global, tetapi tidak menjamin keamanan *thread*. Dalam kasus ini, karena beberapa *thread* dapat mengakses data secara bersamaan, *DashMap* tetap diperlukan untuk menghindari masalah *race condition* dan memastikan akses yang aman.

#### Reflection Publisher-2

1) In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Jawaban:
Dalam pola *Model-View-Controller* (MVC) murni, *Model* bertanggung jawab atas penyimpanan data sekaligus logika bisnis. Namun, dalam praktik pengembangan perangkat lunak yang baik, memisahkan *Service* dan *Repository* dari *Model* sangat disarankan agar kode lebih terstruktur dan mudah dikelola. *Repository* berfokus pada pengelolaan akses data ke database, sedangkan *Service* menangani logika bisnis. Dengan pemisahan ini, kode menjadi lebih modular, mudah diuji, dan lebih fleksibel untuk perubahan di masa depan.  

2) What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?


Jawaban:
Jika hanya menggunakan *Model*, setiap model harus menangani semua aspek, termasuk penyimpanan data, logika bisnis, dan interaksi dengan database. Hal ini akan membuat kode lebih kompleks, sulit dipahami, dan meningkatkan ketergantungan antar *Model*. Sebagai contoh, jika *Program*, *Subscriber*, dan *Notification* harus saling berkomunikasi dalam satu *Model*, perubahan kecil di satu bagian dapat berdampak luas. Dengan adanya *Service* dan *Repository*, interaksi antar *Model* menjadi lebih terstruktur dan lebih mudah dikelola.  

3) Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.


Jawaban:
Ya, saya udah menggunakan Postman untuk menguji *API endpoints* dalam proyek saya. Alat ini sangat membantu dalam memastikan bahwa API berfungsi sesuai harapan sebelum diterapkan dalam sistem. Beberapa fitur yang menurut saya berguna adalah *request testing* untuk mengirim permintaan ke API, *automated testing* untuk mengotomatiskan pengujian API, dan *environment variables* yang memudahkan pengelolaan variabel dalam berbagai lingkungan pengembangan. Fitur-fitur ini sangat berguna untuk proyek kelompok maupun proyek *software engineering* di masa depan.



#### Reflection Publisher-3

1) Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Jawaban: Kode ini menggunakan Push Model. Publisher membuat sebuah Notification dan langsung mengirimkan semua data yang relevan ke setiap subscriber melalui metode update. Subscriber tidak perlu meminta data, mereka hanya menerima apa yang dikirimkan oleh publisher.

2) What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jawaban: 

Kelebihan: Dengan Pull Model, observer memiliki fleksibilitas untuk menentukan kapan mereka ingin memeriksa pembaruan. Ini berguna dalam situasi di mana beberapa observer mungkin sedang tidak aktif atau hanya berjalan ketika pengguna sedang menggunakan aplikasi. Alih-alih menerima pembaruan secara instan, mereka dapat mengambilnya saat aplikasi dibuka kembali, sehingga tidak ada notifikasi yang terlewat. Selain itu, pendekatan ini dapat mengurangi beban pemrosesan dan lalu lintas jaringan, karena tidak semua observer harus diberi tahu secara bersamaan setiap kali terjadi perubahan. Dengan demikian, sistem menjadi lebih skalabel karena tanggung jawab pembaruan didistribusikan ke masing-masing observer, bukan hanya dibebankan pada publisher.

Kekurangan: Kita harus memastikan pemilihan interval atau mekanisme pemicu yang tepat bagi observer untuk melakukan pull. Jika intervalnya terlalu pendek, akan terjadi permintaan pembaruan yang terlalu sering dan tidak perlu, menyebabkan peningkatan penggunaan bandwidth serta beban server. Sebaliknya, jika interval terlalu panjang atau ada perubahan yang terjadi tepat setelah pull, observer bisa tertinggal dari keadaan terbaru sistem dan berpotensi melewatkan pembaruan penting.

3) Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jawaban: 

Jika multi-threading tidak digunakan, proses notifikasi akan berjalan secara berurutan, di mana setiap subscriber diperbarui satu per satu. Hal ini dapat menyebabkan keterlambatan jika ada subscriber yang membutuhkan waktu lama untuk merespons, sehingga memperlambat pembaruan ke subscriber lainnya. Seiring bertambahnya jumlah subscriber, kinerja dan responsivitas sistem akan semakin menurun. Dengan menggunakan multi-threading, setiap subscriber dapat menerima notifikasi secara paralel, yang meningkatkan kecepatan dan skalabilitas sistem.