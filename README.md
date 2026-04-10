# JWT Express TypeORM

A secure REST API boilerplate built with TypeScript, Express.js, and TypeORM. It provides JWT-based authentication, role-based access control, and user management out of the box -- ready to serve as the foundation for any backend application that needs auth.

---

## Features

- JWT-based authentication with login and token refresh
- Password hashing using bcrypt
- Role-based access control (RBAC) with `ADMIN` and `USER` roles
- Full CRUD operations for user management (admin only)
- Password change endpoint for authenticated users
- Input validation using class-validator decorators
- TypeORM entities with auto-sync to SQLite database
- Database migration support with a seed migration for the initial admin user
- Security hardened with Helmet and CORS middleware
- Hot-reload development server with ts-node-dev

## Tech Stack

- **Runtime:** Node.js
- **Language:** TypeScript
- **Framework:** Express.js
- **ORM:** TypeORM 0.2
- **Database:** SQLite
- **Authentication:** JSON Web Tokens (jsonwebtoken)
- **Password Hashing:** bcryptjs
- **Validation:** class-validator
- **Security:** Helmet, CORS
- **Dev Tools:** ts-node-dev, TypeScript compiler

## Project Structure

```
jwt-express-typeorm/
|-- src/
|   |-- index.ts                  # Application entry point, Express server setup
|   |-- config/
|   |   |-- config.ts             # JWT secret and configuration
|   |-- controllers/
|   |   |-- AuthController.ts     # Login and change-password logic
|   |   |-- UserController.ts     # CRUD operations for users
|   |-- entity/
|   |   |-- User.ts               # TypeORM User entity (username, password, role)
|   |-- middlewares/
|   |   |-- checkJwt.ts           # JWT verification middleware
|   |   |-- checkRole.ts          # Role authorization middleware
|   |-- migration/
|   |   |-- 1549202832774-CreateAdminUser.ts  # Seed migration for default admin
|   |-- routes/
|       |-- index.ts              # Route aggregator
|       |-- auth.ts               # Auth routes (login, change-password)
|       |-- user.ts               # User CRUD routes (admin-protected)
|-- ormconfig.json                # TypeORM database configuration
|-- tsconfig.json                 # TypeScript compiler options
|-- package.json                  # Dependencies and scripts
|-- database.sqlite               # SQLite database file
```

## Getting Started

### Prerequisites

- Node.js (v10 or higher recommended)
- npm

### Installation

```bash
git clone https://github.com/akshay1389/jwt-express-typeorm.git
cd jwt-express-typeorm
npm install
```

### Run Database Migrations

Create the default admin user by running the seed migration:

```bash
npm run migration:run
```

### Start the Development Server

```bash
npm start
```

The server starts on **http://localhost:3000** with hot-reload enabled.

### Build for Production

```bash
npm run tsc
npm run prod
```

## Usage

### API Endpoints

**Authentication**

| Method | Endpoint               | Description              | Auth Required |
|--------|------------------------|--------------------------|---------------|
| POST   | `/auth/login`          | Login with credentials   | No            |
| POST   | `/auth/change-password`| Change current password  | Yes (JWT)     |

**User Management (Admin Only)**

| Method | Endpoint      | Description          | Auth Required       |
|--------|---------------|----------------------|---------------------|
| GET    | `/users`      | List all users       | Yes (JWT + ADMIN)   |
| GET    | `/users/:id`  | Get user by ID       | Yes (JWT + ADMIN)   |
| POST   | `/users`      | Create a new user    | Yes (JWT + ADMIN)   |
| PATCH  | `/users/:id`  | Edit a user          | Yes (JWT + ADMIN)   |
| DELETE | `/users/:id`  | Delete a user        | Yes (JWT + ADMIN)   |

### Example: Login

```bash
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin"}'
```

The response includes a JWT token. Include it in subsequent requests:

```bash
curl http://localhost:3000/users \
  -H "Authorization: Bearer <your-token>"
```

### Default Admin Credentials

After running migrations, the default admin account is:
- **Username:** admin
- **Password:** admin

Change the password immediately after first login.

## License

This project is open source. See the repository for license details.
