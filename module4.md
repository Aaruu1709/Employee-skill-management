# Module 4: Skill Entity + OneToMany Mapping + Cascade + Orphan Removal

---

# Business Requirement

One employee can have multiple skills.

Example:

```text
Arti

Java
Spring Boot
Microservices
Docker
AWS
```

Relationship:

```text
Employee (1)
        ↓
Skill (Many)
```

---

# Entity Design

## Employee

```java
@OneToMany(
        mappedBy = "employee",
        cascade = CascadeType.ALL,
        orphanRemoval = true
)
@Builder.Default
private List<Skill> skills = new ArrayList<>();
```

---

## Skill

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "employee_id")
private Employee employee;
```

---

# What We Learned

* @OneToMany
* @ManyToOne
* mappedBy
* @JoinColumn
* CascadeType.ALL
* orphanRemoval
* FetchType.LAZY
* Parent-Child Relationship
* Foreign Key Mapping

---

# @OneToMany

## Why did we use it?

One employee can have multiple skills.

Example:

```text
Arti
 ↓
Java
Spring Boot
Docker
AWS
```

---

## Interview Ready Answer

> I used `@OneToMany` because one employee can have multiple skills. This relationship helps maintain normalized data and provides better scalability and maintainability.

---

## Trap Question

### Why not ManyToMany?

### Ready Answer

> In our project, a skill belongs to only one employee, so OneToMany is sufficient. If skills were shared globally among multiple employees, then ManyToMany would be more appropriate.

---

# @ManyToOne

## Why did we use it?

Many skills belong to one employee.

```text
Java
Spring Boot
Docker
        ↓
      Arti
```

---

## Interview Ready Answer

> `@ManyToOne` represents the child side of the relationship, where multiple skill records are associated with a single employee.

---

# mappedBy

```java
mappedBy = "employee"
```

This refers to:

```java
private Employee employee;
```

inside the Skill entity.

---

## Why did we use mappedBy?

To avoid creating an unnecessary join table.

Without mappedBy:

```text
employees

skills

employee_skills ❌
```

Hibernate creates an extra table.

---

## Interview Ready Answer

> `mappedBy` tells Hibernate that the relationship is already managed by the employee field in the Skill entity, so an additional join table should not be created.

---

## Trap Question

### What happens if we remove mappedBy?

### Ready Answer

> Hibernate creates an additional join table because it assumes both entities are independently managing the relationship.

---

# @JoinColumn

```java
@JoinColumn(name = "employee_id")
```

---

## Why did we use it?

To create a foreign key:

```text
employee_id
```

inside the skills table.

---

## Interview Ready Answer

> `@JoinColumn` specifies the foreign key column that establishes the relationship between Skill and Employee entities.

---

# CascadeType.ALL

```java
cascade = CascadeType.ALL
```

---

## What does it mean?

Example:

```java
Employee employee = new Employee();

employee.getSkills().add(javaSkill);

employeeRepository.save(employee);
```

Hibernate automatically saves:

```text
Employee
 ↓
Skills
```

No need:

```java
skillRepository.save(skill);
```

---

## Interview Ready Answer

> Cascade allows parent operations to propagate automatically to child entities. CascadeType.ALL includes persist, merge, remove, refresh, and detach operations.

---

## Operations Included in CascadeType.ALL

```text
PERSIST
MERGE
REMOVE
REFRESH
DETACH
```

---

## Trap Question

### What operations are included in CascadeType.ALL?

### Ready Answer

> CascadeType.ALL includes PERSIST, MERGE, REMOVE, REFRESH, and DETACH operations.

---

# orphanRemoval = true

```java
orphanRemoval = true
```

---

## Example

Initially:

```text
Arti

Java
Spring Boot
Docker
```

Now:

```java
employee.getSkills().remove(javaSkill);
```

Hibernate automatically executes:

```sql
DELETE FROM skills
WHERE id = 1;
```

---

## Interview Ready Answer

> Orphan removal automatically deletes child records when they are removed from the parent's collection, which helps maintain data consistency.

---

# VERY IMPORTANT TRAP QUESTION

## Difference Between Cascade REMOVE and orphanRemoval

### Ready Answer

> Cascade REMOVE deletes child entities when the parent itself is deleted. Orphan removal deletes a child entity when it is removed from the parent's collection, even if the parent still exists.

---

# FetchType.LAZY

```java
@ManyToOne(fetch = FetchType.LAZY)
```

---

## Why did we use LAZY loading?

When we fetch a skill:

```text
Java
```

we don't always need:

```text
Complete Employee Object
```

Loading only when required improves performance.

---

## Interview Ready Answer

> I prefer LAZY loading because related entities are fetched only when needed, which reduces unnecessary database queries and improves application performance.

---

## Trap Question

### Why not EAGER?

### Ready Answer

> EAGER loading fetches related data immediately, which may lead to unnecessary queries and performance issues. I use EAGER only when related data is always required, such as roles in Spring Security authentication.

---

# Project Design Decision

Why did we choose:

```text
Employee
        ↓
OneToMany
        ↓
Skill
```

instead of:

```text
Employee
        ↓
ManyToMany
        ↓
Skill
```

---

## Interview Ready Answer

> In our current business requirement, skills belong to a specific employee profile. If the organization maintained a global skill catalog shared among employees, we could redesign it using a ManyToMany relationship.

---

# Database Structure

```text
employees
----------------
id
first_name
last_name
...


skills
----------------
id
skill_name
experience_years
employee_id
```

---

# Module 4 Summary

## What We Learned

* OneToMany Mapping
* ManyToOne Mapping
* mappedBy
* JoinColumn
* Foreign Keys
* Cascade Operations
* Orphan Removal
* LAZY Fetching
* Parent-Child Relationships

---

# Interview Questions

1. Why did you use OneToMany?
2. Why not ManyToMany?
3. What is mappedBy?
4. What happens if mappedBy is removed?
5. What is CascadeType.ALL?
6. Which operations are included in CascadeType.ALL?
7. What is orphanRemoval?
8. Difference between Cascade REMOVE and orphanRemoval?
9. Why did you use FetchType.LAZY?
10. When should we use FetchType.EAGER?
11. Why did you use JoinColumn?
12. How is the foreign key created?

---

# Git Commit

```bash
git add .

git commit -m "feat: added skill entity with one-to-many mapping and cascade operations"
```
