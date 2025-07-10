# 🐘 PostgresCrudApp

A simple CRUD API built using **ASP.NET Core 9** and **PostgreSQL**, powered by **Entity Framework Core**.

---

## 🧰 Tech Stack

| Technology                    | Version | Description                       |
|------------------------------|---------|-----------------------------------|
| .NET Core                    | 9.0     | Base framework                    |
| ASP.NET Core Web API         | 9.0     | Backend REST API                  |
| PostgreSQL                   | 15+     | Relational database               |
| Entity Framework Core        | 9.0+    | ORM for database operations       |
| Npgsql.EntityFrameworkCore   | 9.0+    | PostgreSQL EF Core provider       |
| Swagger (Swashbuckle)        | 6.5+    | API documentation and testing     |

---

## 🧱 Project Structure

```bash
PostgresCrudApp/
├── Controllers/              # API controllers
│   └── UsersController.cs
├── Models/                   # Data models
│   └── User.cs
├── Data/                     # EF DbContext
│   └── ApplicationDbContext.cs
├── Migrations/               # EF migrations
├── Program.cs                # App entrypoint & configuration
├── appsettings.json          # DB connection string
└── README.md                 # Project documentation
```

---

## ⚙️ Getting Started

### 1️⃣ Clone the Repo

```bash
git clone https://github.com/your-username/PostgresCrudApp.git
cd PostgresCrudApp
```

### 2️⃣ Set the Connection String

In `appsettings.json`, update:

```json
"ConnectionStrings": {
  "DefaultConnection": "Host=localhost;Port=5432;Database=YourDb;Username=postgres;Password=yourpassword"
}
```

### 3️⃣ Apply EF Migrations

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### 4️⃣ Run the Application

```bash
dotnet run
```

Visit Swagger UI:  
🔗 `https://localhost:5001/swagger`

---

## 🔁 API Endpoints

| Method | Endpoint        | Description            |
|--------|------------------|------------------------|
| GET    | `/api/users`     | Get all users          |
| GET    | `/api/users/{id}`| Get user by ID         |
| POST   | `/api/users`     | Create new user        |
| PUT    | `/api/users/{id}`| Update existing user   |
| DELETE | `/api/users/{id}`| Delete a user          |

---

## 👤 User Model

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
    public string Skill { get; set; } = string.Empty;
}
```

---

## 🚀 UsersController Example

```csharp
[Route("api/[controller]")]
[ApiController]
public class UsersController : ControllerBase
{
    private readonly ApplicationDbContext _context;

    public UsersController(ApplicationDbContext context)
    {
        _context = context;
    }

    [HttpGet]
    public async Task<ActionResult<IEnumerable<User>>> GetUsers() =>
        await _context.Users.ToListAsync();

    [HttpGet("{id}")]
    public async Task<ActionResult<User>> GetUser(int id)
    {
        var user = await _context.Users.FindAsync(id);
        return user == null ? NotFound() : user;
    }

    [HttpPost]
    public async Task<ActionResult<User>> PostUser(User user)
    {
        _context.Users.Add(user);
        await _context.SaveChangesAsync();
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }

    [HttpPut("{id}")]
    public async Task<IActionResult> PutUser(int id, User user)
    {
        if (id != user.Id) return BadRequest();
        _context.Entry(user).State = EntityState.Modified;
        await _context.SaveChangesAsync();
        return NoContent();
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteUser(int id)
    {
        var user = await _context.Users.FindAsync(id);
        if (user == null) return NotFound();
        _context.Users.Remove(user);
        await _context.SaveChangesAsync();
        return NoContent();
    }
}
```

---

## 📘 Swagger UI

Once app is running, open browser:

```txt
https://localhost:5001/swagger
```

You can test all endpoints directly via Swagger UI.

---

## 🔐 JWT Support (Optional)

For secured endpoints with JWT:

```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

Then configure in `Program.cs`.

---

## 🔑 BCrypt (Optional - Password Hashing)

Add package:

```bash
dotnet add package BCrypt.Net-Next
```

Use like this:

```csharp
string hash = BCrypt.Net.BCrypt.HashPassword("your_password");
bool isValid = BCrypt.Net.BCrypt.Verify("your_password", hash);
```

---

## 📄 License

This project is licensed under the MIT License.

---

## 👨‍💻 Author

Built with ❤️ by [@Malik-Ubaidullah](https://github.com/Malik-Ubaidullah)
