# Spring Boot Course
[//]: <> (275 @OneToMany)

## Chapter
- [Quick start](#start-a-project)
- [REST controller](#create-a-rest-controller)
- [Overview](#overview)
- [Maven](#maven)
  - [pom](#pom-file)
- [Spring Boot starters](#spring-boot-starters)
- [Dev Tools](#dev-tools)
- [Actuator](#actuator)
- [run from CLI](#run-from-the-command-line)
- [Spring Core](#spring-core)
    - [Dependency Inversion Principle](#dependency-inversion-principle)
    - [Dependency Injection](#dependency-injection)
    - [Component Scanning](#component-scanning)
    - [Setter Injection](#setter-injection)
    - [Primary Annotation](#primary-annotation)
    - [Lazy Initialization](#lazy-initialization)
    - [Bean Scope](#bean-scope)
    - [Bean Lifecycle Methods](#bean-lifecycle-methods)
-[Hibernate/JPA overview](#hibernate-jpa)

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

### pom file

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
info.app.deszcription=A super Cool App
info.app.version=1.0.0
```

|Name|Description|
|:--:|:--:|
|/auditevents|Audit events for your app|
|/beans|List of all beans registered in Spring App context|
|/mappings|List of all @RequestMapping paths|
|...||

[Full list](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator.endpoints)

By default only the `/health` is exposed, you can expose all with the wildcard :

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

### Dependency Inversion Principle

> prefer this :

```txt
------------     ---------<I>-      -----------------
- Employee -  →  - DBService -   ←  - DBServiceImpl -
------------     -------------      -----------------
```

> instead of :

```txt
------------     -------------
- Employee -  →  - DBService -
------------     -------------
```

### Dependency injection

 <ins>Implementation details must depend on the high level notions.</ins>

Outsourcing construction and management of object. The Spring Container works like a Factory. The clients
delegates to another object / to a container, the responsibility of a providing its dependencies.

:heavy_exclamation_mark: <ins>**Purpose is to separate technical code from business code.**</ins>

**🛈 Autowiring** <ins>implements</ins> `Dependency injection`.

- Create & manages objects (inversion of control)
- Inject object dependencies (Dependencies Injection)

### Component Scanning

- `@SpringBootApplication` ↦ **Contains following annotations :**
  
  |Annotation|Description|
  |:--:|:--:|
  |`@EnableAutoConfiguration`|Enables Spring Boot's auto-conf support|
  |`@ComponentScan`|Enables component scans sub-packages|
  |`@Configuration`|Able to register extra beans with `@Bean` or import other configuration class|
  
```java
...
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MyDemoApplicatrion {

  public static void main(String[] args) {
    SpringApplication.run(MyDemoApplicatrion, args);
  }
}
// Behind the scenes, creates application context, registers all beans & starts embedded server
```
🛈 By default, Spring Boot starts component scanning & automatically component scans package and sub-packages

|Annotation|Description|
|:--:|:--:|
|`@SpringApplication`|Bootstrap the Spring Boot Application|

If you want SpringBoot scan other packages with another ArtifactId you can specify the location in the annotation :
```java
...
@SpringBootApplication(
    scanBasePackages= {"com.ArtifactId",
                       "com.anotherArtifacId",
                       "com.yetAnotherArtifacId"})
public class BlogApplication {
  ...
}
```
### Setter Injection

**Recommended by the Spring.io team**

|What ? |When ? |
|:--:|:--:|
|Constructor Injection :|Use when you have required dependencies|
|Setter Injection :|Use when you have optional dependencies|

**Not recommended **

|What ? |When ? |
|:--:|:--:|
|Field Injection :|Makes the code harder to unit test|

But if you have more than one class which implements the same interface, wich one you have to choose ?

```java
...
import org.springframework.stereotype.Component;

@Component
public class TennisCoach implements Coach {

  @Override
  public String getDailyWorkout() {
    return "Practice Tennis for 20 minutes";
  }
}
```

```java
import org.springframework.stereotype.Component;

@Component
public class BasketBallCoach implements Coach {

  @Override
  public String getDailyWorkout() {
    return "Practice Basketball for 20 minutes";
  }
}
```
### Qualifier
You can be specific with the `@Qualifier`

```java
...
@RestController
public class DemoController {

  private Coach myCoach;

  @Autowired
  public DemoController(@Qualifier("tennisCoach") Coach theCoach) {
  // above we specify the bean id: tennisCoach, same name as class in camelCase
    myCoach = theCoach;
  }

  @GetMapping("/dailyworkout")
  public String getDailyWorkout() {

    return myCoach.getDailyWorkout();
  }
}
```

### Primary Annotation

Alternate solution is to use the `@Primary` annotation

```java
...
import org.springframework.stereotype.Component;

@Component
@Primary
public class TennisCoach implements Coach {

  @Override
  public String getDailyWorkout() {
    return "Practice Tennis for 20 minutes";
  }
}
```
And the controller became :

```java
...
@RestController
public class DemoController {

  private Coach myCoach;

  @Autowired
  public DemoController(Coach theCoach) {
  // above, now no need for @Qualifier
    myCoach = theCoach;
  }

  @GetMapping("/dailyworkout")
  public String getDailyWorkout() {

    return myCoach.getDailyWorkout();
  }
}
```

**⚠️** You can't have multiple class with the `@Primary` annotation

`@Primary` has priority but you can mix with `@Qualifier` but `@Qualifier` has the higher priority !

### Lazy Initialization

Instead of creating all beans up front, we can specify lazy initialization; bean will only initialized in the following cases :

- It si needed for a dependency
- It is explicitly requested

> Add the @Lazy annotation to a given class and it won't be created

If you want to make it globally in `application.properties` :

```application.properties
spring.main.lazy-initialization=true
```
**+** Advantages :

Only create objects as needed, may help with faster startup time,

**⁻** Disadvantages :

If you have web related components like `@RestController`, not created until requested, may not discover configuration
issue until too late and verify you have enough memory for all beans once created.

> Lazy initialization feature is disabled by default.

### Bean Scope

⚠ The default scope in spring is **`Singleton`**.
<ins>All the dependency injections for the bean, will reference the same bean !</ins>

<ins>Example :</ins>

When instanciating with [`@Qualifier`](#qualifier) :

```java
@RestController
public class DemoController {
  private Coach myCoach;
  private Coach anotherCoach;

  @Autowired
  public DemoController(
                @Qualifier("tennisCoach") Coach theCoach,
                @Qualifier("tennisCoach) Coach theOtherCoach) {
    myCoach = theCoach;
    otherCoach = theOtherCoach;
  }
...
}
```
⚠ <ins>Both point the same instance</ins> ⚠

Additional Spring Bean scopes :

|Scope|Description|
|:--:|:--:|
|singleton|Create a single shared instance of the bean. Default scope.|
|prototype|Creates a new bean instance for each container request|
|request|Scoped to an HTTP web request. Only used for web apps.|
|session|Scoped to an HTTP web session. Only used for web apps.|
|global-session|Scoped to a global HTTP web session. Only used for web apps.|

**Prototype Scope Example**

New object instance for each injection. Points two different areas of memory, beans.

```java
import org.springframework.beans.factory.config.ConfigurableBeanFactory;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public class TennisCoach implements Coach {
  ...
}
```
### Bean Lifecycle Methods

When the container started : 

```txt
             ------------------      -------------------------     ---------------------------   ----------------
Container  → - Bean Instiated -   →  - Dependencies injected -  →  - Internal Spring Process - → - Your  Method -
 started     ------------------      -------------------------     ---------------------------   ----------------
                                                 ↓
                                       Container Is Shutdown
                                                 ↓
                                ------------------------------   -------- 
                                - Your Custom destroy Method - → - STOP -
                                ------------------------------   --------
```

You can add custom during bean initialazation, calling custom business logic methods,
setting up handles to resources (db, socket, files etc.). You can also add custom
code during bean destruction.

```java
@Component
public class TennisCoach implements Coach {
  public TennisCoach() {

    System.out.println("In constructor : " + getClass().getSimpleName();
  }

  @PostContruct
  public void doMyStartUpStuff() {
    System.out.println("In doMyStartUpStuff : " + getClass().getSimpleName();
  }
...
}
```
You can do the same for `@PreDestroy`.

> For "prototype" scoped beans, Spring does not call the destroy method.
> 
> In contrast to the other scopes, Spring does not manage the complete
> lifecycle of a prototype bean
> Te container instantiates, configures, and otherwise assembles a
> prototype object, and hands it to the client, with no further record
> of that prototype instance.
>
> The client code must clean up prototype-scoped objects and release
> expensive resources that the prototype bean(s) are holding.

Difference between `@Component` and doing a configuration with `@Configuration` is mainly for the complexity
of your config for a simple class `@Component` is sufficient

## Hibernate JPA

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
