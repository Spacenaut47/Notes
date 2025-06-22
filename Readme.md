# 🧑‍💼 Employee Admin Portal - .NET 8 Web API with Entity Framework Core (CRUD)

A complete ASP.NET Core Web API project built on **.NET 8**, demonstrating **CRUD operations** using **Entity Framework Core** and **SQL Server**.

---

## 🔧 Tech Stack

- .NET 8 SDK  
- ASP.NET Core Web API  
- Entity Framework Core  
- SQL Server (Developer/Express)  
- Swagger (OpenAPI)

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/EmployeeAdminPortal.git
cd EmployeeAdminPortal
```

### 2. Install NuGet Packages
```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

---

## 🏗 Project Structure

```
EmployeeAdminPortal/
├── Controllers/
│   └── EmployeesController.cs
├── Data/
│   └── ApplicationDbContext.cs
├── Models/
│   ├── DTOs/
│   │   ├── AddEmployeeDto.cs
│   │   └── UpdateEmployeeDto.cs
│   └── Entities/
│       └── Employee.cs
├── Program.cs
├── appsettings.json
```

---

## 🔨 Database Setup

### 1. Configure Connection String
```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=EmployeesDb;Trusted_Connection=True;TrustServerCertificate=True;"
}
```

### 2. Run Migrations
```powershell
Add-Migration InitialCreate
Update-Database
```

> ⚠ If you see globalization errors, update `.csproj`:
```xml
<InvariantGlobalization>false</InvariantGlobalization>
```

---

## 🧱 Entity Model

### `Employee.cs`
```csharp
public class Employee
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
    public string? Phone { get; set; }
    public decimal Salary { get; set; }
}
```

---

## 🧾 DTOs

### AddEmployeeDto.cs
```csharp
public class AddEmployeeDto
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string? Phone { get; set; }
    public decimal Salary { get; set; }
}
```

### UpdateEmployeeDto.cs
```csharp
public class UpdateEmployeeDto
{
    public string Name { get; set; }
    public string Email { get; set; }
    public string? Phone { get; set; }
    public decimal Salary { get; set; }
}
```

---

## 🗃 DbContext

### ApplicationDbContext.cs
```csharp
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }

    public DbSet<Employee> Employees { get; set; }
}
```

### Register in Program.cs
```csharp
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 📡 EmployeesController

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    private readonly ApplicationDbContext dbContext;

    public EmployeesController(ApplicationDbContext context)
    {
        dbContext = context;
    }

    [HttpGet]
    public IActionResult GetAllEmployees() =>
        Ok(dbContext.Employees.ToList());

    [HttpGet("{id}")]
    public IActionResult GetEmployeeById(Guid id)
    {
        var employee = dbContext.Employees.Find(id);
        return employee == null ? NotFound() : Ok(employee);
    }

    [HttpPost]
    public IActionResult AddEmployee(AddEmployeeDto dto)
    {
        var employee = new Employee
        {
            Name = dto.Name,
            Email = dto.Email,
            Phone = dto.Phone,
            Salary = dto.Salary
        };
        dbContext.Employees.Add(employee);
        dbContext.SaveChanges();
        return Ok(employee);
    }

    [HttpPut("{id}")]
    public IActionResult UpdateEmployee(Guid id, UpdateEmployeeDto dto)
    {
        var employee = dbContext.Employees.Find(id);
        if (employee == null) return NotFound();

        employee.Name = dto.Name;
        employee.Email = dto.Email;
        employee.Phone = dto.Phone;
        employee.Salary = dto.Salary;

        dbContext.SaveChanges();
        return Ok(employee);
    }

    [HttpDelete("{id}")]
    public IActionResult DeleteEmployee(Guid id)
    {
        var employee = dbContext.Employees.Find(id);
        if (employee == null) return NotFound();

        dbContext.Employees.Remove(employee);
        dbContext.SaveChanges();
        return Ok();
    }
}
```

---

## 🧪 Test with Swagger

- Run the app
- Go to: `https://localhost:<port>/swagger`
- Try:
  - `GET /api/employees`
  - `POST /api/employees`
  - `GET /api/employees/{id}`
  - `PUT /api/employees/{id}`
  - `DELETE /api/employees/{id}`

---

## 📊 CRUD Summary

| Operation | Route                    | Method | Description           |
|-----------|--------------------------|--------|-----------------------|
| Create    | `/api/employees`         | POST   | Add new employee      |
| Read All  | `/api/employees`         | GET    | Get all employees     |
| Read One  | `/api/employees/{id}`    | GET    | Get employee by ID    |
| Update    | `/api/employees/{id}`    | PUT    | Update employee       |
| Delete    | `/api/employees/{id}`    | DELETE | Delete employee       |

---

## ✅ Next Steps (Optional)

- 🔐 Add JWT Authentication
- 🗂 Use Repository Pattern + Unit of Work
- ⚙ Integrate AutoMapper
- 📦 Deploy with Docker or Azure

---

## 🙌 Support

If this project helped you, give it a ⭐ and share it!

