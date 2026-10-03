# Student Management System

A full-stack **Student Management System** built with **ASP.NET Core MVC, Entity Framework Core, SQL Server LocalDB, REST API, and Swagger**. The project demonstrates a clean layered architecture with the **Repository Pattern, Service Layer, Dependency Injection, CRUD operations, search, sorting, pagination, and soft delete**.

## Project Overview

The application provides a web-based interface for managing student records. Users can create, view, update, search, sort, and soft-delete students through Razor Views. The same student data is also exposed through a REST API documented with Swagger/OpenAPI.

The project was developed as a practical demonstration of ASP.NET Core MVC and backend development concepts relevant to a **Graduate Engineer Trainee (GET) / Fresher Software Developer** role.

## Key Features

- Student CRUD operations
- Create, read, update, and soft-delete student records
- Search students by student number, name, email, or course
- Sorting by student number, first name, email, course, year level, and last name
- Pagination for student lists
- Server-side validation using ViewModels and ModelState
- Duplicate student-number and email validation
- Soft delete using an `IsActive` flag
- Repository Pattern for database access
- Service Layer for business logic
- Dependency Injection
- Entity Framework Core with SQL Server
- RESTful API endpoints
- Swagger/OpenAPI documentation
- Global exception-handling middleware
- Asynchronous database operations with `async`/`await`
- Responsive Razor-based UI

## Technology Stack

| Category | Technology |
|---|---|
| Language | C# |
| Framework | ASP.NET Core 8 MVC |
| ORM | Entity Framework Core 8 |
| Database | SQL Server LocalDB |
| Frontend | Razor Views, HTML, CSS, Bootstrap |
| API | ASP.NET Core Web API / REST |
| API Documentation | Swagger / OpenAPI |
| Architecture | MVC + Service Layer + Repository Pattern |
| Dependency Injection | Built-in ASP.NET Core DI |
| Version Control | Git & GitHub |
| IDE | Visual Studio |

## Architecture

The application follows a layered architecture to keep presentation, business logic, and data-access responsibilities separated.

```text
                         Browser
                            |
                            v
                  +---------------------+
                  |   MVC Controller    |
                  | StudentsController  |
                  +----------+----------+
                             |
                             v
                  +---------------------+
                  |    Service Layer    |
                  |   StudentService    |
                  +----------+----------+
                             |
                             v
                  +---------------------+
                  | Repository Interface|
                  | IStudentRepository   |
                  +----------+----------+
                             |
                             v
                  +---------------------+
                  | Repository Layer    |
                  | StudentRepository   |
                  +----------+----------+
                             |
                             v
                  +---------------------+
                  | Entity Framework    |
                  | ApplicationDbContext|
                  +----------+----------+
                             |
                             v
                  +---------------------+
                  | SQL Server LocalDB  |
                  | StudentManagementDb |
                  +---------------------+
```

### Main Responsibilities

**Controller**
- Handles HTTP requests and responses
- Performs model binding and ModelState validation
- Selects Razor Views or redirects
- Delegates application work to the service layer

**Service Layer**
- Contains business logic
- Checks duplicate student numbers and emails
- Maps entities to DTOs
- Coordinates operations between controllers and repositories

**Repository Layer**
- Contains Entity Framework Core database operations
- Handles querying, insertion, updating, and soft deletion
- Implements searching, sorting, and pagination

**ApplicationDbContext**
- Represents the database session through Entity Framework Core
- Provides the `Students` DbSet
- Configures database mappings and indexes

## Database

The application uses SQL Server LocalDB with the following connection string for local development:

```json
"DefaultConnection": "Server=(localdb)\\MSSQLLocalDB;Database=StudentManagementDb;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True"
```

### Students Table

| Column | Type | Description |
|---|---|---|
| StudentId | uniqueidentifier | Primary key |
| StudentNumber | nvarchar(20) | Unique student number |
| FirstName | nvarchar(100) | Student first name |
| LastName | nvarchar(100) | Student last name |
| Email | nvarchar(150) | Unique student email |
| Course | nvarchar(100) | Course/program |
| YearLevel | int | Student year level |
| DateCreated | datetime2 | Record creation date |
| DateUpdated | datetime2 | Last update date |
| IsActive | bit | Active/soft-deleted status |

The database also contains indexes for student number, email, last name/first name, and course to support common queries.

## CRUD Operations

### Create

A user enters student information through the Create Student form. The controller validates the ViewModel, the service checks business rules such as duplicate student number/email, and the repository saves the entity using Entity Framework Core.

### Read

The Students page retrieves active students from the database and supports search, sorting, and pagination.

### Update

The Edit operation loads an existing student, validates the submitted data, checks uniqueness rules, updates the entity, and saves the changes.

### Delete

The project uses **soft delete** rather than physically removing a row:

```csharp
student.IsActive = false;
```

Normal queries filter with `IsActive = true`, so deleted students no longer appear in the normal application flow while the database record remains available.

## REST API

Swagger is available in Development at:

```text
https://localhost:5001/swagger
```

### Student API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/students` | Get students |
| POST | `/api/students` | Create a student |
| GET | `/api/students/{id}` | Get a student by ID |
| PUT | `/api/students/{id}` | Update a student |
| DELETE | `/api/students/{id}` | Delete a student |

The API can be tested directly through Swagger UI without requiring a separate API client.

## Entity Framework Core & Migrations

The project uses EF Core migrations to manage database schema changes.

The initial migration creates the `StudentManagementDb` database and `Students` table.

To apply migrations:

```powershell
Update-Database
```

Or using the .NET CLI:

```bash
dotnet ef database update
```

## How to Run the Project

### Prerequisites

Install:

- Visual Studio 2022/2026 with ASP.NET and web development workload
- .NET 8 SDK
- SQL Server LocalDB
- Git

### 1. Clone the repository

```bash
git clone https://github.com/AbhishekPawar-1904/StudentManagement-ASP.NET-Core.git
cd StudentManagement-ASP.NET-Core
```

### 2. Restore dependencies

```bash
dotnet restore
```

### 3. Update the database

Open the Package Manager Console in Visual Studio and run:

```powershell
Update-Database
```

### 4. Build the application

```bash
dotnet build
```

### 5. Run the application

```bash
dotnet run
```

Then open the HTTPS URL shown by ASP.NET Core in the browser.

## Project Structure

```text
StudentManagement/
│
├── Controllers/
│   ├── Api/
│   │   └── StudentsApiController.cs
│   ├── HomeController.cs
│   └── StudentsController.cs
│
├── Data/
│   ├── Repositories/
│   │   └── StudentRepository.cs
│   └── ApplicationDbContext.cs
│
├── Interfaces/
│   ├── IStudentRepository.cs
│   └── IStudentService.cs
│
├── Middleware/
│   └── GlobalExceptionMiddleware.cs
│
├── Migrations/
│
├── Models/
│   ├── Student.cs
│   └── ErrorViewModel.cs
│
├── Services/
│   └── StudentService.cs
│
├── ViewModels/
│   ├── StudentDto.cs
│   ├── StudentIndexViewModel.cs
│   └── StudentViewModel.cs
│
├── Views/
│   ├── Home/
│   ├── Shared/
│   └── Students/
│
├── wwwroot/
├── appsettings.json
├── Program.cs
└── StudentManagement.csproj
```

## Interview-Relevant Concepts Demonstrated

This project demonstrates practical understanding of:

- ASP.NET Core MVC
- MVC architecture
- Controllers and routing
- Razor Views
- Model binding
- ModelState validation
- Dependency Injection
- Service Layer
- Repository Pattern
- Entity Framework Core
- DbContext and DbSet
- LINQ
- SQL Server
- REST API design
- HTTP methods: GET, POST, PUT, DELETE
- Async/Await
- Middleware
- Exception handling
- DTOs and ViewModels
- Entity Framework Core migrations
- Search, sorting, and pagination
- Soft delete
- Git and GitHub

## Screenshots

### Dashboard

Add a project screenshot here, for example:

```text
screenshots/dashboard.png
```

### Student List

```text
screenshots/students-list.png
```

### Swagger API

```text
screenshots/swagger.png
```

> To add screenshots, create a `screenshots` folder in the repository, place the images inside it, and replace the placeholders above with Markdown image links such as `![Dashboard](screenshots/dashboard.png)`.

## Learning Outcomes

Through this project, I practiced building a complete ASP.NET Core application from database layer to UI and REST API. The project helped strengthen my understanding of clean separation of concerns, dependency injection, Entity Framework Core, CRUD operations, database migrations, API development, and Git-based project management.

## Author

**Abhishek Ambalal Pawar**

B.Tech Computer Engineering

R.C. Patel Institute of Technology, Shirpur

GitHub: [AbhishekPawar-1904](https://github.com/AbhishekPawar-1904)

---

⭐ If you find this project useful, feel free to explore the code and API documentation.
