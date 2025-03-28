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
    -   [x] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [X] Commit: `Implement add function in Subscriber repository.`
    -   [X] Commit: `Implement list_all function in Subscriber repository.`
    -   [X] Commit: `Implement delete function in Subscriber repository.`
    -   [X] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
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


### 1. **Do we need an interface (trait) in this BambangShop case, or is a single Model struct enough?**
In the Observer pattern, an interface (or trait in Rust) is useful when there are multiple subscriber types with different behaviors that need to conform to a common contract. However, in the case of BambangShop, if all subscribers share the same behavior and structure, then using a single Model struct is sufficient.

A trait would only be necessary if we anticipated different types of subscribers (e.g., email subscribers, webhook subscribers, etc.) that needed different implementations for the `update` method. Since the current implementation only involves one type of subscriber, adding a trait would add unnecessary complexity without any real benefit.


### 2. **Is using `Vec` (list) sufficient for storing unique IDs, or is `DashMap` necessary?**
Using `Vec` would not be ideal in this case because it does not provide efficient lookups for unique identifiers. In a `Vec`, checking for uniqueness or retrieving a subscriber would require iterating through the entire list, which has a time complexity of **O(n)**. As the number of subscribers grows, this becomes inefficient.

On the other hand, `DashMap` (a concurrent HashMap) allows for **O(1) lookups**, insertions, and deletions, making it more efficient when dealing with a growing list of unique identifiers such as program IDs or subscriber URLs. Additionally, `DashMap` is thread-safe, which is crucial in a multi-threaded web application where multiple users might subscribe or unsubscribe simultaneously.

Thus, **DashMap is necessary** because it provides both **efficient lookups** and **thread safety** without requiring manual synchronization.


### 3. **Do we still need DashMap for thread safety, or can we implement a Singleton pattern instead?**
While a **Singleton pattern** ensures a single instance of `SUBSCRIBERS`, it does not inherently provide **thread safety** for concurrent reads and writes. If we were to implement our own thread-safe Singleton, we would need to wrap a HashMap with **Mutex** or **RwLock**, which could introduce potential performance bottlenecks due to locking overhead.

`DashMap`, on the other hand, **already provides a thread-safe concurrent HashMap** with fine-grained locking, meaning multiple threads can read and write to different keys simultaneously without blocking each other. This makes `DashMap` a more efficient choice compared to manually managing locks in a Singleton-based implementation.

Thus, while we are technically using a Singleton (`lazy_static` ensures `SUBSCRIBERS` is initialized only once), **DashMap remains necessary for efficient, concurrent access** to the subscriber list.
#### Reflection Publisher-2

#### Reflection Publisher-3
