# Spring Boot Course
[//]: <> (275 @OneToMany)

## Chapter
- [Quick start](#start-a-project)
- [REST controller](#create-a-rest-controller)
- [Overview](#overview)
- [Maven](#maven)
  - [pom](#pom.xml)
- [Spring Boot starters](#spring-boot-starters)
- [Dev Tools](#dev-tools)
- [Actuator](#actuator)
- [run from CLI](#run-from-the-command-line)
- [Custom application properties](#custom-application-properties)

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

## Maven

<ins>Builds your Java project and manages dependencies</ins>.

Maven handle class / build path for you.

Standard directory structure :
```txt
🞃 my-cool-app
  pom.xml
  🞃 src
    🞃 main
      🞃 java
        🞃 group
          🞃 artifactId
            🞃 appName
              🞂 controller
              🞂 entity
            AppNameApplication.java
      🞃 resources
        🞂 static
        🞂 templates
      🞂 webapp
    🞃 test
      🞂 java
        🞃 group
          🞃 artifactId
            🞃 appName
              🞂 controller
              🞂 entity
            AppNameApplicationTest.java
      🞂 resources
  🞂 target
```

|Directory|Description|
|:---:|:---:|
|src/main/java|Your Java source code|
|src/main/resources|Properties/config files used by your app|
|src/main/webapp|JSP files & web config or assets (images, css, js etc.)|
|src/test|Unit testing code and properties|
|target|Destination directory for compiled code|

### pom.xml

- Project meta data
  
Project name, version, output files type: JAR,WAR etc.
  
-  dependencies

List of projects we depend on, Spring, Hibernate, etc.

- plug ins

Additional custom tasks to run → generate JUnit test reports etc.

### Exploring generated files

- mvnw → Maven Wrapper Files, allows you to run a Maven project.
  If the correct version of Maven is not found on the computer

  - mvnw.cmd → to launch commands `> mvnw clean compile test` for `Windows`
  - mvnw.sh → to launch commands `./mvnw clean compile test` for `Linux` / `Mac`

 If you have Maven insatlled previously, you can ignore/delete the `mvnw` files & just use Maven as usual :

 ```bash
# example 
mvn clean compile test
```

- pom file (read above)

- Java Source Code
  
  Main Spring Boot aplication class, created by initializr and your controllers

- `resources/applications.properties` to add custom properties or config.
  
  ```aplication.properties
  # configure my props
  coach.name=Mickey Mouse
  ```
  ```java
  @RestController
  public class MyController {
  
    @Value("${coach.name}")
    private String coachName;
  
    ...
  }
  ```

  **⚠** Do not use the `src/main/webapp` if your aplication is packaged as a `JAR` **⚠** Only works with WAR packaging.

## Spring Boot starters

- A curated list of Maven dependencies.
- A collection of dependencies grouped together
- Tested and verified by the Spring Developement Team
- Makes it much easier for the developper to get started
- Reduces the amount of Maven configuration

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
<!-- A collection of Maven dependencies -->
  <artifactId>spring-boot-start-web</artifactId>
<!-- contains spring-web spring-webmvc hibernate-validator json tomcat ... -->
</dependency>
```

In spring initializr just select `Spring Web`, there are 30+ starters from the spring deveopment team

|Name|Description|
|:---:|:---:|
|spring-boot-starter-web|Building web apps, includes validation, REST, uses Tomcat as default|
|spring-boot-starter-security|Adding Spring Security Support|
|spring-boot-starter-jpa|Spring database suport with JPA & Hibernate|
|...||

Benefits of the Spring boot Starter Parent :

- Default Maven config
- Dependency management
  - use version on parent only
  - `spring-boot-starter-*` dependencies inherit version from parent
- Default configuration of Spring Boot plugin

## Dev Tools

Plug-in autmatically restarts your application when code is updated, simply add dependenciy to the POM file.

## Actuator

Exposes endpoints to monitor and manage the application, easyly get DevOps out-of-the-box, simply add dependency to the
POM file. Rest endpoints are automatically added to your application. Endpoints are prefixed with : `/actuator`

`localhost:8080/actuator/health` check the status of your application.

```json
{"status": "UP"}
```

To expose `localhost:8080/actuator/info` :

```aplication.properties
management.endpoints.web.exposure.include=health,info
management.info.env.enabled=true
info.app.name=My app
info.app.deszcriotion=A super Cool App
info.app.version=1.0.0
```

|Name|Description|
|:--:|:--:|
|/auditevents|Audit events for your app|
|/beans|List of all beans registered in Spring App context|
|/mappings|List of all @RequestMapping paths|
|...||

[Full list](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator.endpoints)

By defauklt only the `/health` is exposed, you can expose all with the wildcard :

```aplication.poperties
management.endpoints.web.exposure.include=*
```

```xml
<!-- add support to actuator -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

### Exclude Endpoints

```aplication.properties
# Exclude individual endpoints
management.endpoints.web.exposure.exclude=health,info
```

[More details](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready)

### Securing Endpoints

```xml
<!-- add support to security -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<!-- password is in console -->
```

## Run from the command line

Option 1 : Use `java -jar my-app.jar`

Option 2 : Use Spring Boot Maven plugin

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
</dependency>
```

## Spring Core

### Inversion of Control

Outsourcing construction and management of object. The Spring Container works like a Factory. The clients
delegates to another object the responsibility of a providing its dependencies.

**🛈Autowiring** <ins>implements</ins> `Dependency injection`.

- Create & manages objects (inversion of control)
- Inject object dependencies (Dependencies Injection)



## Lexicon

- HATEOAS :  Hypermedia as the Engine of Application State
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
- pom file → Project Object Model file
- GAV : Group ID, Artifact Id, Version
