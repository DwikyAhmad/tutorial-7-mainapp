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
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Subscriber model struct.`
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [X] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [X] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [X] Commit: `Implement publish function in Program service and Program controller.`
    -   [X] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [X] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Kebutuhan sebuah interface untuk Subscriber bergantung pada tujuan pembuatan aplikasi pada masa yang akan mendatang, apabila aplikasi kemungkinan akan dilakukan scaling menjadi subscriber yang memiliki sifat yang berbeda-beda. Maka, interface akan diperlukan untuk mempermudah development. Namun, karena bambangShop hanya memiliki satu Subscriber class saja, maka satu model struct saja sudah cukup.

2. Dalam case id dan url Subscriber yang unik, maka penggunaan DashMap akan diperlukan karena sifatnya yang memiliki key identifier yang unik dan juga kemudahan mengelola data secara efisien jika dibandingkan menggunakan Vec (list) yang dapat menyimpan data yang duplikat.

3. Menurut saya, penggunaan DashMap masih tetap diperlukan karena aplikasi bambangShop yang akan berjalan secara multithreading, membutuhkan sebuah koleksi data yang dapat diakses dengan aman oleh multiple thread karena sifatnya yang thread safe jika dibandingkan dengan Singleton pattern yang membatasi instance class menjadi satu instance saja, membuat implementasi menjadi kompleks akibat sifat rust yang memiliki ownership dan borrowing model.

#### Reflection Publisher-2
1. Pemisahan antara Service dan Repository diperlukan untuk memenuhi Single Responsibility Principle. Pada MVC, model memiliki fungsionalitas untuk mengelola data storage dan business logic, hal ini melanggar SRP yang di mana setiap modul seharusnya memiliki fungsionalitas yang terpisah dan independent. Hal ini juga bertujuan untuk mempermudah proses pengembangan dengan meningkatkan maintainability dari kode.

2. Jika hanya menggunakan model, maka aplikasi akan kemungkinan setiap model akan memiliki coupling yang tinggi karena tidak adanya pemisahan service dan repository, membuat maintainability kode menjadi rendah karena sifatnya yang memerlukan perubahan tambahan di area kode lain ketika suatu fitur di model dilakukan perubahan.

3. Postman sangat berguna untuk melakukan testing response pada API yang telah kita buat, postman dapat memberikan input simulasi sebuah user melakukan HTTP requests terhadap aplikasi kita dan kita dapat memonitor hasil yang diberikan dari aplikasi kita untuk mengetahui apakah aplikasi kita telah berjalan sesuai yang diinginkan. Fitur yang saya suka dari postman adalah postman collections yang bisa menyimpan kumpulan http requests yang telah kita buat dan tersusun dalam folder yang membuat testing API menjadi lebih mudah dan efisien.

#### Reflection Publisher-3
1. Pada tutorial ini, variasi yang digunakan adalah Push Model, hal ini dapat dilihat dari algoritma kodingan yang melakukan iterasi send data (notify) ke setiap Subscriber pada setiap operasi CRUD pada product.

2. Keuntungan yang dimiliki dengan Pull model adalah Observer dapat menentukan data apa yang mereka terima dan kapan mereka akan menerima, hal ini memberikan ruang kontrol yang lebih pada Subscriber untuk penerimaan data mereka. Kerugian dari Pull model ini adalah kompleksitas yang meningkat karena harus mengimplementasikan fetch logic dari subject apabila dibandingkan dengan push model yang hanya perlu mengirim update kepada setiap subscriber.

3. Apabila kita tidak menggunakan multi-threading, maka akan terjadi sebuah antrean panjang pada push notifikasi ke subscriber apabila terdapat subscriber dengan skala yang besar, membuat bottleneck dan memperlambat kinerja aplikasi.