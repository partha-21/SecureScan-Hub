# SecureScan Hub

**Production-ready Spring Boot Security Scanning Platform**

---

## Quick Start

### Prerequisites
- Java 17+
- Maven 3.8+
- MySQL 8.0+

### 1. Database Setup
```sql
CREATE DATABASE securescanhub;
```
> The app auto-creates tables via `spring.jpa.hibernate.ddl-auto=update`

### 2. Configure Credentials
Edit `src/main/resources/application.properties` if needed:
```properties
spring.datasource.username=root
spring.datasource.password=Partha_2106
```

### 3. Run the Application
```bash
cd SecureScanHub
mvn clean install -DskipTests
mvn spring-boot:run
```

### 4. Open in Browser
Navigate to: **http://localhost:8080**

---

## Project Structure
```
SecureScanHub/
├── src/main/java/com/securescanhub/
│   ├── controller/          # REST + View Controllers
│   ├── service/             # Business Logic
│   ├── repository/          # Spring Data JPA Repositories
│   ├── model/
│   │   ├── entity/          # JPA Entities (User, ScanHistory)
│   │   └── dto/             # Request/Response DTOs
│   ├── security/
│   │   ├── jwt/             # JWT Token Utility
│   │   ├── filter/          # JWT Authentication Filter
│   │   └── config/          # Security Config, UserDetailsService
│   ├── util/                # AES Encryption, SecurityContext Helper
│   └── exception/           # Global Exception Handler + Custom Exceptions
├── src/main/resources/
│   ├── templates/           # Thymeleaf HTML pages
│   ├── static/css/          # Stylesheet
│   ├── static/js/           # Frontend JavaScript
│   └── application.properties
└── pom.xml
```

---

## API Endpoints

| Method | Endpoint              | Auth Required | Description              |
|--------|-----------------------|---------------|--------------------------|
| POST   | `/auth/register`      |  NULL         | Register new user         |
| POST   | `/auth/login`         |  NULL         | Login, receive JWT token  |
| POST   | `/scan/malware`       |  JWT          | Scan text for malware     |
| POST   | `/scan/malware/file`  |  JWT          | Upload file for scanning  |
| POST   | `/scan/crawler`       |  JWT          | Crawl URL with threads    |
| GET    | `/scan/history`       |   JWT         | Fetch user scan history   |

---

## API Usage (cURL)

### Register
```bash
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com","password":"secret123","securityPin":"1234"}'
```

### Login
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"secret123"}'
```

### Malware Scan (use token from login)
```bash
curl -X POST http://localhost:8080/scan/malware \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"textInput":"This file contains a trojan virus payload"}'
```

### Crawler Scan
```bash
curl -X POST http://localhost:8080/scan/crawler \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'
```

---

## Security Features
- **JWT Authentication** — Stateless, token-based auth
- **BCrypt** — Password hashing with strength 10
- **AES-128** — Security PIN encrypted at rest
- **CSRF Disabled** — Appropriate for REST APIs
- **Role-based Access** — USER and ADMIN roles
- **Global Exception Handling** — Consistent error responses
- **Input Validation** — `@Valid` annotations on all DTOs

---

## Tech Stack
| Layer      | Technology                         |
|------------|------------------------------------|
| Framework  | Spring Boot 3.2.5                  |
| Security   | Spring Security 6 + JWT (JJWT)     |
| Database   | MySQL 8 + Spring Data JPA          |
| Frontend   | Thymeleaf + Vanilla JS             |
| Build      | Maven                              |
| Java       | Java 17                            |
| Utilities  | Lombok, SLF4J                      |

---

## Default Credentials
Register via `/register` page or the API. No default admin is seeded.

To create an ADMIN user, manually update the role in MySQL:
```sql
UPDATE users SET role = 'ADMIN' WHERE email = 'admin@example.com';
```
