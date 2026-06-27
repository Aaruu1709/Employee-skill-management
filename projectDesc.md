# Employee Skill Management System

## Module 1: Project Setup

### Dependencies Added

### 1. Spring Web

**Why did we use it?**

To create REST APIs and handle HTTP requests.

**Interview Answer:**

> I used Spring Web to build REST APIs. It provides Spring MVC, embedded Tomcat, DispatcherServlet, and automatic JSON conversion using Jackson.

---

### 2. Spring Data JPA

**Why did we use it?**

To perform database operations using ORM instead of writing JDBC code.

**Interview Answer:**

> I used Spring Data JPA because it reduces boilerplate code and provides built-in CRUD operations, pagination, sorting, and custom query support.

**Trap Question:**

Why JPA instead of JDBC?

**Answer:**

> JDBC requires a lot of manual code for connections, queries, and object mapping. JPA provides ORM features and reduces development effort.

---

### 3. MySQL Driver

**Why did we use it?**

To connect our Spring Boot application with the MySQL database.

**Trap Question:**

Why do we need MySQL Driver if we already use JPA?

**Answer:**

> JPA provides ORM functionality, but it still needs a JDBC driver to communicate with the actual database. The MySQL driver acts as a bridge.

---

### 4. Lombok

**Why did we use it?**

To reduce boilerplate code like getters, setters, constructors, and builders.

**Interview Answer:**

> Lombok improves code readability by generating boilerplate code at compile time.

**Trap Question:**

Any disadvantages of Lombok?

**Answer:**

> Generated code is not directly visible, so debugging can sometimes be slightly harder. Developers also need Lombok plugin support in their IDE.

---

### 5. Validation

**Why did we use it?**

To validate incoming request data before it reaches business logic.

**Interview Answer:**

> Validation helps reject invalid input at the API layer, improving data integrity and reducing unnecessary processing.

---

### 6. Spring Security

**Why did we use it?**

For authentication and authorization using JWT and role-based access.

---

### 7. DevTools

**Why did we use it?**

For automatic application restart during development.

---

### 8. Spring Boot Test

**Why did we use it?**

For unit testing and integration testing using JUnit and Mockito.

---

# Module 2: Database Setup & Employee Entity

## Database Configuration

### spring.jpa.hibernate.ddl-auto=update

**Interview Answer:**

> During development, I use ddl-auto=update so Hibernate can automatically synchronize entity changes with the database schema. In production, I prefer Flyway or Liquibase.

**Trap Question:**

Why not use create in production?

**Answer:**

> create or create-drop can remove existing data. Production schema changes should be controlled using migration tools.

---

### spring.jpa.show-sql=true

**Interview Answer:**

> I enable show-sql during development to verify generated SQL queries and debug performance issues.

---

# EmployeeStatus Enum

```java
ACTIVE
INACTIVE
ON_LEAVE
```

**Why Enum instead of String?**

**Interview Answer:**

> Enums provide type safety, prevent invalid values, and improve readability for fixed values like employee status.

**Trap Question:**

How do you store enums in the database?

**Answer:**

> I use EnumType.STRING because it is more readable and safer than ordinal values.

---

# Employee Entity

## @Entity

**Interview Answer:**

> @Entity tells JPA that this class represents a database table and should be managed by the persistence context.

**Trap Question:**

Can Hibernate create a table without @Entity?

**Answer:**

> No. Without @Entity, Hibernate does not treat the class as a persistent object.

---

## @Table

**Interview Answer:**

> I use @Table when I want to customize the table name or define table-level configurations.

---

## @Id

**Interview Answer:**

> @Id marks the primary key field that uniquely identifies each record.

---

## @GeneratedValue(strategy = GenerationType.IDENTITY)

**Interview Answer:**

> I used IDENTITY because MySQL supports auto-increment columns, allowing the database to generate unique IDs.

**Trap Question:**

Difference between IDENTITY and SEQUENCE?

**Answer:**

> IDENTITY uses database auto-increment, while SEQUENCE uses a separate sequence object. PostgreSQL and Oracle commonly use sequences.

---

## @Column

**Interview Answer:**

> @Column allows us to define constraints such as uniqueness, nullability, and custom column names.

**Trap Question:**

Is @Column mandatory?

**Answer:**

> No, it is optional. JPA provides default mappings, but @Column gives additional control.

---

## @Enumerated(EnumType.STRING)

**Interview Answer:**

> I prefer EnumType.STRING because it improves readability and avoids issues if enum order changes in future.

---

# Module 3: Role Entity & Many-To-Many Mapping

## Why Separate Role Entity?

We intentionally created a Role table instead of storing roles as strings.

Benefits:

* Better normalization
* Scalable design
* Easy integration with Spring Security
* Multiple roles per employee

---

# RoleType Enum

```java
ROLE_ADMIN
ROLE_HR
ROLE_EMPLOYEE
```

**Interview Answer:**

> I use Enums for roles because roles are fixed values and Enums provide type safety and maintainability.

---

# @ManyToMany

**Interview Answer:**

> @ManyToMany is used when both entities can have multiple relationships with each other. An employee can have multiple roles, and a role can belong to multiple employees.

---

### Trap Question

Why ManyToMany and not OneToMany?

**Answer:**

> One employee can have multiple roles, and one role can belong to multiple employees, so ManyToMany is the natural relationship.

---

### Trap Question

Why not store roles as comma-separated strings?

**Answer:**

> Storing comma-separated values breaks normalization and makes querying and maintenance difficult. A separate Role entity is more scalable.

---

# @JoinTable

**Interview Answer:**

> @JoinTable defines the intermediate table that manages the many-to-many relationship between Employee and Role.

---

# @JoinColumn

**Interview Answer:**

> @JoinColumn specifies the foreign key column used to establish relationships between entities.

---

# FetchType.EAGER

**Interview Answer:**

> I used EAGER fetching because Spring Security needs role information immediately during authentication and authorization.

---

### Trap Question

Why can EAGER fetching be dangerous?

**Answer:**

> EAGER loading may fetch unnecessary data and increase query complexity, which can affect performance in large systems.

---

# Project Design Decisions Till Now

## Why Layered Architecture?

**Interview Answer:**

> Layered architecture improves separation of concerns, maintainability, testing, and scalability.

---

## Why DTO Pattern?

**Interview Answer:**

> DTOs prevent exposing internal entities, avoid sensitive data leakage, and keep API contracts independent from database changes.

---

## Why Service and ServiceImpl?

**Interview Answer:**

> Service provides abstraction, while ServiceImpl contains business logic. This improves loose coupling and future extensibility.

---

## Trap Question

Why create an interface if there is only one implementation?

**Answer:**

> Interfaces support loose coupling, easier unit testing, and future enhancements without affecting dependent classes.
