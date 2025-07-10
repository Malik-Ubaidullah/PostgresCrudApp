# 🐘 PostgresCrudApp

A simple CRUD API using ASP.NET Core 9 and PostgreSQL with Entity Framework Core.

---

## 🛠️ Tech Stack

| Technology                       | Version  | Purpose                              |
|----------------------------------|----------|--------------------------------------|
| .NET Core                        | 9.0      | Framework                            |
| ASP.NET Core Web API             | 9.0      | Backend REST API                     |
| PostgreSQL                       | 15+      | Relational database                  |
| Entity Framework Core            | 9.0+     | ORM for DB access                    |
| Npgsql.EntityFrameworkCore       | 9.0+     | PostgreSQL provider for EF           |
| Swagger (Swashbuckle)            | 6.5+     | API documentation                    |

---

## 📂 Project Structure

```bash
PostgresCrudApp/
│
├── Controllers/              # API Controllers
│   └── UsersController.cs
│
├── Models/                   # Data Models
│   └── User.cs
│
├── Data/                     # Database Context
│   └── ApplicationDbContext.cs
│
├── Migrations/               # EF Migrations
│
├── Program.cs                # App Configuration
├── appsettings.json          # DB Connection String
└── README.md                 # Project Documentation

⚙️ Prerequisites
.NET SDK 9.0

PostgreSQL

A PostgreSQL client like pgAdmin or DBeaver

🚀 Getting Started
🔧 Clone & Restore

git clone https://github.com/your-username/PostgresCrudApp.git
cd PostgresCrudApp
dotnet restore

🧩 Configure Connection String
Update your appsettings.json:

"ConnectionStrings": {
  "DefaultConnection": "Host=localhost;Port=5432;Database=YourDbName;Username=postgres;Password=yourpassword"
}

🏗️ Apply Migration

dotnet ef migrations add InitialCreate
dotnet ef database update

▶️ Run the App

dotnet run

Now visit: https://localhost:5001/swagger

🔁 API Endpoints

| Method | Endpoint        | Description             |
| ------ | --------------- | ----------------------- |
| GET    | /api/users      | Get all users           |
| GET    | /api/users/{id} | Get a user by ID        |
| POST   | /api/users      | Create a new user       |
| PUT    | /api/users/{id} | Update an existing user |
| DELETE | /api/users/{id} | Delete a user           |


🧠 Model Example

public class User
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
    public string Skill { get; set; } = string.Empty;
}

📘 Sample Controller Logic

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
    public async Task<ActionResult<IEnumerable<User>>> GetUsers()
    {
        return await _context.Users.ToListAsync();
    }

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

🧪 Swagger UI
Once you run the app, open your browser at:

https://localhost:5001/swagger
You can test all API endpoints from Swagger directly.

🔐 JWT Support (Optional)
To enable JWT-based authentication:

dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
And configure it in Program.cs.

🧂 BCrypt (Optional for Passwords)

dotnet add package BCrypt.Net-Next

Use it like:
string hashed = BCrypt.Net.BCrypt.HashPassword("password");
bool isValid = BCrypt.Net.BCrypt.Verify("password", hashed);

📃 License
Licensed under MIT License

👨‍💻 Author
Built with ❤️ by @Malik-Ubaidullah


---
