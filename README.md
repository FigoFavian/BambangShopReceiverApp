# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create SubscriberRequest model struct.`
    -   [✓] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Notification repository.`
    -   [✓] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [✓] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Commit: `Implement receive_notification function in Notification service.`
    -   [✓] Commit: `Implement receive function in Notification controller.`
    -   [✓] Commit: `Implement list_messages function in Notification service.`
    -   [✓] Commit: `Implement list function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

Answer:

1. We use RwLock<> to synchronize access to the Vec of Notifications to ensure concurrency while maintaining safety. This allows multiple threads to read the notifications simultaneously when calling list_all_as_string, while only one thread can gain exclusive access to modify the list using add. This brief blocking ensures safe modification without affecting concurrent reads. On the other hand, using Mutex<> would be inefficient because it locks the entire structure for both reading and writing, meaning even a single read operation would block all other threads, leading to unnecessary delays.

2. Unlike Java, where static variables can be mutated using static functions without restriction, Rust does not allow direct mutation of static variables due to its strict thread safety enforcement. Rust prevents data races and undefined behavior by limiting mutability in static variables. To work around this, we use the lazy_static external library, which allows static variables to be initialized at runtime and then used safely as mutable data while maintaining Rust's safety guarantees.

#### Reflection Subscriber-2

Answers:

1. The lib.rs file encapsulates shared components, configuration, and error handling, setting the stage for the application’s overall structure. It defines the compose_error_response function to standardize error handling, and it establishes a global HTTP client via REQWEST_CLIENT to promote efficiency by avoiding repetitive reinitializations. Moreover, the file uses APP_CONFIG—which is initialized from environment variables using dotenvy and Figment—to set up application settings, while the AppConfig structure stores values like instance_root_url, publisher_root_url, and instance_name with auto-generated getter methods from getset. Ultimately, I did explore beyond the tutorial steps, delving into the lib.rs file to gain insights into these shared functionalities.

2. Each instance of the Main app, when spawned, independently handles its own set of notifications, ensuring that registered subscribers receive notifications according to their subscribed product types. This is made possible by the Observer pattern, which leverages Rust traits to support diverse subscriber behaviors and adheres to the Open-Closed Principle for better maintainability and flexibility. In essence, the Observer pattern streamlines the integration of more subscribers, regardless of whether it's a single or multiple instances of the Main app.

3. The testing process becomes more efficient because of Postman. Which it simplifies sending and viewing HTTP requests for various endpoints. In addition to the convenience offered by Postman, implementing unit tests for every component has allowed me to ensure that each part of the system behaves as expected. This dual approach not only minimizes repetitive manual testing but also enhances overall testing efficiency for both my tutorial work and group projects.