# laravel-api-baseline

Laravel 11 API baseline with Docker, Sanctum, Swagger, Pint, Larastan and testing setup ready to use.

---

## What's included

- **Laravel 11** — PHP framework
- **PostgreSQL** — relational database via Docker
- **Docker** — PHP 8.3, Nginx, PostgreSQL 16 containerized
- **Laravel Sanctum** — token-based authentication ready
- **Swagger / OpenAPI** — interactive documentation + `make:swagger` command
- **Laravel Pint** — code formatting with Laravel preset
- **Larastan** — static analysis at level 5
- **SQLite in-memory** — isolated test environment via `.env.testing`

---

## Folder Structure

```
app/
├── Console/Commands/     ← custom Artisan commands (make:swagger)
├── Enums/                ← application enums
├── Events/               ← domain events
├── Http/
│   ├── Controllers/Api/  ← API controllers
│   ├── Requests/         ← form requests
│   ├── Resources/        ← API resources
│   └── Swagger/          ← Swagger documentation files
├── Listeners/            ← event listeners
├── Mail/                 ← mailables
├── Models/               ← Eloquent models
├── Policies/             ← authorization policies
├── Queries/              ← read/filter layer
└── Services/             ← business logic layer
```

---

## Architecture

```
Request → FormRequest → Controller → Service/Query → Model → Resource → Response
```

| Layer       | Responsibility            |
| ----------- | ------------------------- |
| Controller  | HTTP orchestration        |
| Service     | Write business logic      |
| Query       | Read, filters, pagination |
| FormRequest | Input validation          |
| Policy      | Authorization rules       |
| Resource    | Response transformation   |

---

## Getting Started

### Requirements

- Docker
- Docker Compose

### Setup

```bash
# Clone the repository
git clone https://github.com/RaulJPNeto/laravel-api-baseline.git my-project
cd my-project

# Copy environment file
cp src/.env.example src/.env

# Start containers
docker compose up -d

# Fix permissions
docker compose exec app chown -R www-data:www-data storage bootstrap/cache
docker compose exec app chmod -R 775 storage bootstrap/cache

# Install dependencies
docker compose exec app composer install

# Generate app key
docker compose exec app php artisan key:generate

# Run migrations
docker compose exec app php artisan migrate
```

---

## Development

```bash
# Format code
docker compose exec app composer pint

# Check formatting without applying
docker compose exec app composer pint:test

# Run static analysis
docker compose exec app composer stan

# Format + analyse
docker compose exec app composer check

# Generate Swagger docs
docker compose exec app php artisan l5-swagger:generate

# Generate Swagger docs file for a new controller
docker compose exec app php artisan make:swagger ExampleController

# Run all tests
docker compose exec app php artisan test
```

---

## API Documentation

Interactive documentation available via Swagger UI after starting the project:

```
http://localhost:8000/api/documentation
```

---

## Commit Convention

This project follows [Conventional Commits](https://www.conventionalcommits.org/).

### Format

```
<type>: <short description>
```

### Types

| Type       | When to use                              |
| ---------- | ---------------------------------------- |
| `feat`     | New feature or endpoint                  |
| `fix`      | Bug fix                                  |
| `refactor` | Code change that is not a fix or feature |
| `chore`    | Config, dependencies, tooling            |
| `docs`     | Documentation only                       |
| `test`     | Adding or updating tests                 |
| `style`    | Formatting, missing semicolons, etc.     |

---

## License

[MIT](LICENSE)
