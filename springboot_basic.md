# 🚀 Spring Boot Complete Basic Documentation

Spring Boot is an open-source, Java-based framework built on top of the traditional Spring framework. It helps you create **standalone, production-ready applications** quickly with minimal configuration.

---

## 🧭 Architecture Flow

A typical Spring Boot application follows a layered architecture to separate concerns:

```mermaid
graph TD
    Client["🌐 Client (Browser/Postman)"] -->|1. HTTP Request| Controller["🎮 Controller Layer (@RestController)"]
    Controller -->|2. Calls business logic| Service["⚙️ Service Layer (@Service)"]
    Service -->|3. Database operations| Repository["💾 Repository Layer (@Repository)"]
    Repository -->|4. Queries / Updates| DB[("🗄️ Database")]
    DB -->|5. Data Return| Repository
    Repository -->|6. Entity / DTO| Service
    Service -->|7. Processed Data| Controller
    Controller -->|8. HTTP Response (JSON/XML)| Client

    style Client fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px
    style Controller fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style Service fill:#fffde7,stroke:#fbc02d,stroke-width:2px
    style Repository fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px
    style DB fill:#ffebee,stroke:#f44336,stroke-width:2px
```

---

## ⚡ 1. Key Features of Spring Boot

Spring Boot simplifies enterprise application development through its core philosophy: **"Opinionated Defaults"**.

*   🚀 **Auto Configuration:** Automatically configures your Spring application based on the dependencies (JARs) added to the classpath (e.g., adding `spring-boot-starter-web` automatically configures Tomcat and Spring MVC).
*   🔌 **Embedded Servers:** No need to install and deploy WAR files to external servers. It comes with an embedded Tomcat, Jetty, or Undertow instance.
*   🚫 **No XML Configuration:** Entirely configures your application using Java Annotations and properties/YAML files, removing verbose XML files.
*   📊 **Production-Ready Features:** Out-of-the-box support for monitoring application health, metrics, and env info via **Spring Boot Actuator**.
*   📦 **Easy Dependency Management:** Simplifies build configuration by providing **Starter POMs** (e.g., `spring-boot-starter-data-jpa`).

---

## 🛠️ 2. Creating a Spring Boot Project

### Using Spring Initializr (Recommended)
1. Go to [start.spring.io](https://start.spring.io).
2. Configure the following project metadata:
    *   **Project:** Maven Project
    *   **Language:** Java
    *   **Spring Boot:** Latest stable version
    *   **Packaging:** Jar
    *   **Java:** 17 or 21 (Recommended)
3. **Dependencies to Add:**
    *   `Spring Web` (for REST APIs, MVC)
    *   `Spring Data JPA` (for SQL database connectivity)
    *   `MySQL Driver` (or H2 for local testing)
4. Click **Generate** to download the project zip, extract it, and open it in your IDE.

---

## 📁 3. Project Directory Structure

A typical Spring Boot application follows this standard layout:

```text
myapp/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/myapp/
│   │   │       ├── MyAppApplication.java   <-- Main Entry Point
│   │   │       ├── controller/             <-- API Endpoints
│   │   │       ├── service/                <-- Business Logic
│   │   │       ├── repository/             <-- Database Operations
│   │   │       └── model/                  <-- Entity/Model Classes
│   │   └── resources/
│   │       ├── static/                     <-- Static assets (CSS/JS)
│   │       ├── templates/                  <-- HTML templates (Thymeleaf/Freemarker)
│   │       └── application.properties       <-- Application Configurations
└── pom.xml                                 <-- Maven Dependencies
```

---

## 🏁 4. Main Application Class

The entry point of every Spring Boot application is annotated with `@SpringBootApplication`.

```java
package com.example.myapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyAppApplication {
    public static void main(String[] args) {
        // Launches the embedded server and boots up the Spring Application Context
        SpringApplication.run(MyAppApplication.class, args);
    }
}
```

> [!NOTE]
> `@SpringBootApplication` is a convenience annotation that combines:
> 1. `@SpringBootConfiguration` (declares this class as a source of bean definitions).
> 2. `@EnableAutoConfiguration` (tells Spring Boot to auto-configure components based on classpath dependencies).
> 3. `@ComponentScan` (tells Spring to scan the package of this class and all subpackages for `@Component`, `@RestController`, `@Service`, and `@Repository`).

---

## 🌐 5. Creating a REST Controller

Controllers handle incoming HTTP requests and return the response directly as JSON/XML or text.

```java
package com.example.myapp.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

*   **Test URL:** `http://localhost:8080/api/hello`

---

## 🔄 6. Dependency Injection & Service Layer

Dependency Injection (DI) allows Spring to manage the creation, configuration, and lifecycle of components (beans).

### Step 1: Create the Service Class (Business Logic)
```java
package com.example.myapp.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {
    public String getUser() {
        return "John Doe";
    }
}
```

### Step 2: Inject Service into the Controller (Constructor Injection)
```java
package com.example.myapp.controller;

import com.example.myapp.service.UserService;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    // Final variable ensures immutability
    private final UserService userService;

    // Spring automatically injects the UserService bean via constructor injection
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/user")
    public String getUser() {
        return userService.getUser();
    }
}
```

> [!TIP]
> **Constructor Injection** is the recommended best practice for dependency injection in Spring Boot over `@Autowired` field injection. It supports immutability (`final` fields) and makes testing easier with mocks without spinning up a heavy Spring Container.

---

## ⚙️ 7. Configuration File (`application.properties`)

Located in `src/main/resources/application.properties`, this file manages environment-specific configurations.

```properties
# Change the default server port
server.port=8081

# Application naming
spring.application.name=myapp
```

---

## 🏷️ 8. Essential Spring Boot Annotations

| Annotation | Target Layer | Description |
| :--- | :--- | :--- |
| **`@SpringBootApplication`** | Main class | Marks entry point, enables auto-config, and scans components. |
| **`@RestController`** | Controller | Combines `@Controller` and `@ResponseBody` for REST endpoints. |
| **`@Service`** | Service | Defines business logic beans. |
| **`@Repository`** | Repository | Manages database operations and translates SQL exceptions. |
| **`@Component`** | Utility/General | Generates generic Spring-managed beans. |
| **`@Autowired`** | Dependency Injection | Injects a bean automatically into a field/constructor/setter. |

---

## 📝 9. REST Methods & CRUD Controller Example

### HTTP Methods Mapping

| Method | CRUD Action | HTTP Mapping Annotation |
| :--- | :--- | :--- |
| **GET** | Read | `@GetMapping` |
| **POST** | Create | `@PostMapping` |
| **PUT** | Update | `@PutMapping` |
| **DELETE** | Delete | `@DeleteMapping` |

### Complete CRUD Controller Example
```java
package com.example.myapp.controller;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    // 1. Get all users (Read)
    @GetMapping
    public String getAllUsers() {
        return "Get all users";
    }

    // 2. Create a user (Create)
    @PostMapping
    public String createUser() {
        return "User created";
    }

    // 3. Update a user (Update)
    @PutMapping
    public String updateUser() {
        return "User updated";
    }

    // 4. Delete a user (Delete)
    @DeleteMapping
    public String deleteUser() {
        return "User deleted";
    }
}
```

---

## 📦 10. Building & Running the Application

### Development (Using Maven Wrapper)
To run the application locally in development mode:
```bash
./mvnw spring-boot:run
```

### Production Build & Packaging
To build a single executable JAR file containing all dependencies and the embedded server:
```bash
./mvnw clean package
```
*   **Build Output Artifact:** `target/myapp-0.0.1-SNAPSHOT.jar`

### Running the Packaged Application
Run the executable JAR directly using the Java command:
```bash
java -jar target/myapp-0.0.1-SNAPSHOT.jar
```

---

## 🌐 11. Environments & Profiles

Profiles allow you to define configuration properties for different environments (Dev, Test, Prod).

### Step 1: Create environment-specific files
*   **`application-dev.properties`**
    ```properties
    spring.datasource.url=jdbc:mysql://localhost:3306/devdb
    ```
*   **`application-prod.properties`**
    ```properties
    spring.datasource.url=jdbc:mysql://prod-db-server:3306/proddb
    ```

### Step 2: Run the application with a specific active profile
```bash
java -jar app.jar --spring.profiles.active=dev
```

---

## 🎯 12. Summary

Spring Boot drastically accelerates Java backend development by:
*   Providing **embedded HTTP servers** out of the box.
*   **Automating configuration** tasks to reduce boilerplate configuration.
*   Ensuring structured, **layered architecture** through standard components.
*   Enabling clean **REST API design** using robust annotations.
