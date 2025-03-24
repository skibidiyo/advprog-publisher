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

    In the Observer pattern, a Subscriber is usually a trait that defines how to get updates. In BambangShop, the Subscriber is just a struct with data and no custom behavior. Since all subscribers work the same way, a trait is not needed right now. You only need a trait if different types of subscribers do different things. So for now, using just a struct is enough.

2) id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    Using Vec to store unique items like id or url is possible but not efficient. You would need to manually check for duplicates and search through the list, which gets slower as the list grows. DashMap provides fast access, automatic uniqueness by key, and easy deletion. Using DashMap is the better and more appropriate choice in this case.

3) When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    The Singleton pattern gives you a single, global instance but doesnt make data thread-safe. In Rust, thread safety is important, especially when multiple threads access shared data like SUBSCRIBERS. DashMap is a thread-safe map that allows safe concurrent access without manual locking. Even with a Singleton, you would still need DashMap or another thread-safe structure. So, using DashMap is still necessary in this case.

#### Reflection Publisher-2
1) In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    Separating Service and Repository from Model helps keep the code clean and organized. Each part has its own job: models hold data, services handle logic, and repositories deal with the database. This makes the code easier to understand, test, and change. It also helps our project grow without becoming messy.

2) What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    If we only use the Model, each model would have to do everything : handle data, logic, and database access. This makes the code messy and harder to understand. The models would depend too much on each other, and small changes could break many things. Separating logic into services and repositories keeps things clean and easier to manage.

3) Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    Yes, Ive used Postman to test my API endpoints, which helps me quickly check if they work as expected. Its a useful tool for both solo and group projects, especially when working with APIs.


#### Reflection Publisher-3
1) Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    The code uses the Push Model. The publisher constructs a Notification and pushes all relevant data directly to each subscriber via their update method. Subscribers do not request data, they simply receive what the publisher sends.

2) What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    - Advantage : With the Pull Model, observers have the flexibility to decide when to check for updates. This is useful in scenarios where certain observers may be inactive or only run while the user is actively using the application. Instead of receiving updates instantly, they can fetch them when the app resumes, avoiding missed notifications. It also helps reduce processing load and network traffic, since not all observers are notified simultaneously on every change. This can improve system scalability by distributing the update responsibility to each observer, instead of overloading the publisher with update tasks.

    - Disadvantage : We would have to ensure we choose appropriate intervals or trigger events for when the observer performs a pull. If the interval is too short, it can result in frequent and unnecessary requests for updates, even when nothing has changed, causing extra bandwidth usage and increased server load. If the interval is too long or a change happens right after a pull, the observer might remain out of sync with the server for a while, potentially missing important updates.

3) Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    If multi-threading is not used, the notification process will run sequentially, meaning each subscriber is updated one at a time. This can lead to delays if any subscriber takes too long to respond, blocking the rest of the updates. As the number of subscribers grows, the overall performance and responsiveness of the system will degrade. Using multi-threading avoids these issues by allowing each subscriber to be notified in parallel, improving speed and scalability.