# 🧑‍💼 Employee Management System
**Spring Boot • Spring Data JPA • FreeMarker • MySQL**

---

## 📌 Project Overview
This project is a **web-based Employee Management System** developed using **Spring Boot**, **Spring Data JPA**, **FreeMarker templates**, and **MySQL**. It is designed to manage organizational data efficiently through a clean interface and a well-structured backend.

The system supports full **CRUD (Create, Read, Update, Delete)** functionality for:
- Employees
- Departments
- Attendance records

A clear separation between backend processing and frontend rendering is maintained to improve readability, scalability, and long-term maintenance.

---

## 🏗️ Application Architecture
The system follows a **layered architecture** pattern:

```
Controller → Service → Repository → Database
            ↓
       FreeMarker Views (FTL/FTLH)
```

Each layer has a distinct responsibility, ensuring loose coupling and clean code organization.

---

## 🔧 Core Components

### 1️⃣ Controllers

#### Web Controller
- Manages page routing and navigation
- Passes data to FreeMarker templates using the `Model`
- Controls page access to ensure correct navigation flow

#### REST Controllers
- `EmployeeRestController`
- `DepartmentRestController`
- `AttendanceRestController`

Responsibilities:
- Handle HTTP requests for create, update, and delete actions
- Provide API-style operations separate from view rendering

---

### 2️⃣ Service Layer
Each entity is supported by its own service:

- `EmployeeService`
- `DepartmentService`
- `AttendanceService`

Key responsibilities:
- Apply business rules and validations
- Retrieve and process data from repositories
- Keep controllers lightweight and focused

Service interfaces (e.g., `IEmployeeService`) are used to promote consistency and future extensibility.

---

### 3️⃣ Repository Layer
Repositories extend `JpaRepository`:

- `EmployeeRepository`
- `DepartmentRepository`
- `AttendanceRepository`

Responsibilities:
- Manage all database interactions
- Provide built-in CRUD methods
- Allow custom queries when necessary

---

## 🗂️ Data Models & Relationships

### Employee
- Assigned to a single Department
- Can have multiple Attendance records

Relationships:
- `Many-to-One` with Department
- `One-to-Many` with Attendance

---

### Department
- Can contain multiple Employees

Relationship:
- `One-to-Many` with Employee

---

### Attendance
- Linked to one Employee
- Stores attendance date and status

Relationship:
- `Many-to-One` with Employee

---

### 🔗 Relationship Summary

| Entity Relationship | Type |
|--------------------|------|
| Department → Employee | One-to-Many |
| Employee → Attendance | One-to-Many |
| Attendance → Employee | Many-to-One |

All relationships are implemented using **JPA annotations** to ensure data integrity and proper foreign key management.

---

## 🧭 Navigation & Access Control

### Dashboard
- Serves as the main entry point
- Provides links to all management modules

**URL:** `/dashboard`

---

### List Pages (Primary Access Points)
All CRUD operations must originate from list pages:

| Page | URL |
|------|-----|
| Employee List | `/employees/list` |
| Department List | `/departments/list` |
| Attendance Log | `/attendance/list` |

When a list page is accessed, a session flag is stored to track valid navigation flow.

---

### 🔒 CRUD Access Rules
To prevent direct URL access to forms:
- Add, Edit, and View pages are restricted
- Users must come from a corresponding list page
- Unauthorized access redirects back to the list view

This logic is enforced using a session-based validation method:

```java
private boolean canAccess(HttpSession session)
```

Benefits:
- Controlled user flow
- Cleaner navigation experience
- Prevention of orphaned pages

---

## 🎨 Frontend (FreeMarker Templates)

### Template Structure
Each entity includes the following pages:
- List
- View
- Add
- Edit

Example templates:
- `listEmployees.ftlh`
- `addEmployee.ftlh`
- `editEmployee.ftlh`
- `viewEmployee.ftlh`

---

### 🎨 CSS Organization
The UI styling is modular and reusable:

| CSS File | Purpose |
|--------|---------|
| `main.css` | Global layout, fonts, navigation |
| `list.css` | Tables and list views |
| `view.css` | Read-only detail pages |
| `add.css` | Add forms |
| `update.css` | Edit/update forms |

Each FreeMarker page:
- Always includes `main.css`
- Includes one additional CSS file based on page type

This structure ensures visual consistency while keeping styles easy to maintain.

---

## 🗄️ Database Configuration
- Uses **MySQL** as the database
- Configured via `application.properties`
- Hibernate manages schema updates automatically
- Tables are generated and maintained through JPA

⚠️ The database must be created manually before running the application.

---

✨ *A structured and maintainable Employee Management System built with modern Ja