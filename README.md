# Spring Boot Course
[//]: <> (275 @OneToMany)

## Chapter
- [Quick start](#start-a-project)
- [REST controller](#create-a-rest-controller)
- [Overview](#overview)
- [Maven](#maven)
  - [pom](#pom.xml)

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
