# Spring_Security

A demo project showing how to secure a **Spring Boot** web application
using **Spring Security** with a **custom user details service** backed
by a **MySQL** database (via Spring Data JPA). The app uses **JSP** views
served through Tomcat Jasper for the UI.

---

## Features

- Form-based authentication with Spring Security
- Custom `UserDetailsService` implementation that loads users from MySQL
- User entity persisted via Spring Data JPA (`UserRepository`)
- Role / authority based access control
- JSP views rendered through Tomcat Jasper
- Clear separation of config, controller, service, and repository layers

---

## Tech Stack

- **Java 11**
- **Spring Boot 2.5.2**
- **Spring Security**
- **Spring Web (MVC)**
- **Spring Data JPA**
- **MySQL** (`mysql-connector-java` 5.1.46)
- **JSP** + **Tomcat Jasper** 8.5.37
- **Maven** (Maven Wrapper included)

---

## Project Structure

Spring_Security/
├── src/
│   └── main/
│       ├── java/com/example/demo/
│       │   ├── AppSecurityConfig.java       # Spring Security configuration
│       │   ├── HomeController.java          # Web endpoints / view routing
│       │   ├── MyUserDetails.java           # UserDetails implementation
│       │   ├── MyUserDetailsService.java    # Custom UserDetailsService
│       │   ├── SecurityApplication.java     # Spring Boot entry point
│       │   ├── User.java                    # JPA entity for app users
│       │   └── UserRepository.java          # Spring Data JPA repository
│       ├── resources/                       # application properties, etc.
│       └── webapp/                          # JSP views
├── .gitignore
├── mvnw / mvnw.cmd                          # Maven wrapper
├── pom.xml
└── README.md

---

## Prerequisites

- **JDK 11+**
- **Maven 3.6+** (or use the bundled `mvnw` wrapper)
- **MySQL 5.7+** running locally (or reachable remotely)

---

## Database Setup

Create a database and a `user` table used by the app:

```sql
CREATE DATABASE spring_security_db;

USE spring_security_db;

CREATE TABLE user (
    id            INT AUTO_INCREMENT PRIMARY KEY,
    username      VARCHAR(50)  NOT NULL UNIQUE,
    password      VARCHAR(200) NOT NULL,
    active        INT          NOT NULL DEFAULT 1,
    roles         VARCHAR(100) NOT NULL
);

-- Example user (password should be stored as a BCrypt hash in practice)
INSERT INTO user (username, password, active, roles)
VALUES ('admin', 'admin', 1, 'ROLE_ADMIN');
```

---

## Configuration

Update `src/main/resources/application.properties` with your database
credentials and JSP view settings, for example:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/spring_security_db
spring.datasource.username=root
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# JSP view resolver
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp

server.port=8080
```

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/Spring_Security.git
cd Spring_Security
```

### 2. Build the project

```bash
./mvnw clean install
```

### 3. Run the application

```bash
./mvnw spring-boot:run
```

The app starts on:
http://localhost:8080/

You'll be redirected to Spring Security's login page. Sign in with a
user that exists in the `user` table.

---

## How It Works

- **`SecurityApplication.java`** bootstraps the Spring Boot app.
- **`AppSecurityConfig.java`** configures authentication and
  authorization rules — which URLs are public, which require login,
  and wires in the custom `UserDetailsService`.
- **`MyUserDetailsService.java`** implements Spring Security's
  `UserDetailsService` and looks up users through `UserRepository`.
- **`MyUserDetails.java`** adapts the `User` entity into Spring
  Security's `UserDetails` contract.
- **`User.java` / `UserRepository.java`** define the JPA entity and
  repository used to fetch users from MySQL.
- **`HomeController.java`** exposes the web endpoints and returns JSP
  view names.

---

## Learning Goals

This project is a good reference for:

- Integrating Spring Security into a Spring Boot application
- Implementing a custom `UserDetailsService` backed by a database
- Using Spring Data JPA with MySQL
- Serving JSP views from a Spring Boot app

---

## License

This project is intended for learning and demonstration purposes.
