# Spring Boot Course
[//]: <> (275 @OneToMany)

## Chapter
- [Quick start](#start-a-project)
- [REST controller](#create-a-rest-controller)
- [Overview](#overview)

- [Lexicon](#lexicon)

## Start a project

[Spring Initializr](https://start.spring.io/)

## Create A REST Controller

Create a new folder/package under your the app folder you define in the initializer
(eg : controller), and a new `Class` inside.

`@RestController` → is a specialized version of the controller. It includes the `@Controller` and `@ResponseBody`.

`@GetMapping` → handling HTTP GET requests.

```java
@RestController
public class SimpleRestController {

  @GetMapping("/)
  public String helloWorld() {

    return "Hello World";
  }
}
```
## Overview

The Core Container is a factory for creating beans & manages bean dependencies. Core and beans are responsible for
providing the fundamentals of the framework and dependency injection.

## Lexicon

- Beans : Generic Objects
- POJO : Plain Old Java Object
- AOP : Apect Oriented Programing
- JDBC : Java DataBase Connectivity → Helpers classes to reduce your source code
- OXM : Object XML Mappers
- JMS : Java Message Service → For sending async messages.
- ORM : JPA & Hibernate
- Data Access Layer : to communicate with the database,
    - JDBC, ORM, Transactions, OXM, JMS
- Web Layer :
    - Servlet, WebSocket, Web
- Test Layer :
    - Unit, Integration, Mock
