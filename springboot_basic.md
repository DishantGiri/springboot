# 🚀 Spring Boot Basic Documentation

## 1. What is Spring Boot?
Spring Boot is a framework built on top of Spring that helps build **standalone, production-ready applications** quickly with minimal configuration.

### Key Features
- Auto Configuration  
- Embedded Servers (Tomcat / Jetty)  
- No XML configuration  
- Production-ready features (Actuator)  
- Simplified dependency management  

---

## 2. Creating a Spring Boot Project

### Using Spring Initializr
👉 https://start.spring.io

### Project Setup
- Project: Maven  
- Language: Java  
- Spring Boot: Latest stable version  

### Dependencies
- Spring Web  
- Spring Data JPA  
- MySQL Driver (optional)  

Generate and extract the project.

---

## 3. Project Structure

myapp/
├── src/main/java/com/example/myapp
│ ├── MyAppApplication.java
│ ├── controller/
│ ├── service/
│ ├── repository/
│ └── model/
│
├── src/main/resources
│ └── application.properties
│
└── pom.xml


---

## 4. Main Application Class
```java
@SpringBootApplication
public class MyAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyAppApplication.class, args);
    }
}
5. Creating a REST API
Controller Example
@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}

Test URL:
http://localhost:8080/api/hello

6. Dependency Injection
Service
@Service
public class UserService {
    public String getUser() {
        return "John Doe";
    }
}
Controller
@RestController
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/user")
    public String getUser() {
        return userService.getUser();
    }
}
7. Configuration
application.properties
server.port=8081
spring.application.name=myapp
8. Spring Boot Annotations
Annotation	Description
@SpringBootApplication	Main application class
@RestController	REST API controller
@Service	Business logic layer
@Repository	Database layer
@Component	Generic Spring bean
@Autowired	Dependency injection
9. Running the Application
Development
./mvnw spring-boot:run
Production
java -jar target/myapp.jar
10. Build the Project
./mvnw clean package

Output:

target/myapp-0.0.1-SNAPSHOT.jar
11. REST Methods
Method	Usage
GET	Read data
POST	Create data
PUT	Update data
DELETE	Delete data
12. CRUD Controller Example
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping
    public String getAllUsers() {
        return "Get all users";
    }

    @PostMapping
    public String createUser() {
        return "User created";
    }

    @PutMapping
    public String updateUser() {
        return "User updated";
    }

    @DeleteMapping
    public String deleteUser() {
        return "User deleted";
    }
}
13. Profiles
application-dev.properties
spring.datasource.url=jdbc:mysql://localhost/devdb
Run with profile
java -jar app.jar --spring.profiles.active=dev
14. Summary

Spring Boot helps you:

Build APIs quickly
Reduce boilerplate configuration
Use embedded servers
Focus on business logic instead of setup
