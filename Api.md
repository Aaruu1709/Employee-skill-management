# Module 7: Repository Layer + Service Layer + First POST API

---

## Interview Questions & Answers

### 1. Why did you extend JpaRepository?

**Answer:**

> I extended JpaRepository because it provides built-in CRUD operations, pagination, sorting, and JPA-specific functionalities, reducing boilerplate code and improving development speed.

---

### 2. Difference between CrudRepository, PagingAndSortingRepository, and JpaRepository?

**Answer:**

> CrudRepository provides basic CRUD operations. PagingAndSortingRepository adds pagination and sorting support. JpaRepository extends both and adds JPA-specific methods like flush(), saveAndFlush(), and batch operations.

---

### 3. What are Derived Query Methods?

**Answer:**

> Derived Query Methods are methods where Spring Data JPA automatically generates SQL queries based on method names, such as findByEmail or findByDepartment.

---

### 4. How does Spring create queries without writing SQL?

**Answer:**

> Spring Data JPA analyzes method names and automatically generates the corresponding SQL query at runtime.

---

### 5. Why did you create a Service Interface?

**Answer:**

> I created a Service Interface to achieve loose coupling and follow the Dependency Inversion Principle. It makes the application easier to test, maintain, and extend.

---

### 6. Can we avoid using Service Interfaces?

**Answer:**

> Yes, technically we can, but using interfaces is an industry best practice because it improves flexibility, testability, and maintainability.

---

### 7. What is the Dependency Inversion Principle?

**Answer:**

> The Dependency Inversion Principle states that high-level modules should depend on abstractions rather than concrete implementations. In Spring Boot, Service Interfaces help us achieve this principle.

---

### 8. Why did you use @Service?

**Answer:**

> @Service marks a class as a business layer component and allows Spring to automatically detect and manage it as a bean.

---

### 9. Difference between @Component and @Service?

**Answer:**

> Both create Spring beans, but @Service provides semantic meaning that the class belongs to the business layer, making the code more readable and maintainable.

---

### 10. Why did you use constructor injection?

**Answer:**

> Constructor injection makes dependencies immutable, improves testability, and avoids NullPointerExceptions. It is the recommended approach in Spring Boot applications.

---

### 11. Why not use field injection with @Autowired?

**Answer:**

> Field injection makes unit testing difficult and hides dependencies. Constructor injection explicitly shows required dependencies and supports immutability.

---

### 12. Why did you use @RequiredArgsConstructor?

**Answer:**

> @RequiredArgsConstructor automatically generates constructors for final fields, reducing boilerplate code and encouraging constructor injection.

---

### 13. What is @Valid?

**Answer:**

> @Valid triggers Bean Validation annotations such as @NotBlank, @Email, and @Size before the request reaches the business layer.

---

### 14. What happens if @Valid is removed?

**Answer:**

> Validation annotations will not execute automatically, and invalid data may enter the application.

---

### 15. Why did you use ResponseEntity?

**Answer:**

> ResponseEntity allows us to control the HTTP status code, headers, and response body, making REST APIs more flexible and standards-compliant.

---

### 16. Why did you return HTTP 201 CREATED for POST APIs?

**Answer:**

> According to REST standards, a successful resource creation should return HTTP 201 CREATED instead of HTTP 200 OK.

---

### 17. Why did you get 401 Unauthorized?

**Answer:**

> Spring Security secures all endpoints by default. Since authentication was not configured yet, the APIs returned 401 Unauthorized.

---

### 18. Why did you disable CSRF?

**Answer:**

> I disabled CSRF because our application exposes stateless REST APIs. When using JWT authentication, CSRF protection is generally not required since authentication is token-based.

---

### 19. What is CSRF?

**Answer:**

> CSRF (Cross-Site Request Forgery) is an attack where a malicious website tricks a user's browser into sending unauthorized requests using the user's existing session. It mainly affects session-based authentication systems.

---

### 20. Why do we use Repository Layer?

**Answer:**

> The Repository Layer separates database access logic from business logic, making the application cleaner, easier to maintain, and easier to test.

---
