# Service Scheduling System (Backend API) 🌟

A Spring Boot RESTful API backend for the **Service Scheduling System**, designed to manage service offerings, appointment bookings, client reviews, and company-client interactions. Built to integrate seamlessly with single-page frontend applications such as **Angular**.

Cloned from: [Hanna-nigussie/Service-Scheduling-System](https://github.com/Hanna-nigussie/Service-Scheduling-System.git)  
Author: **Hanna Nigussie (UGR/9180/14)**

---

## Architecture Overview 🛠️

The backend operates as a decoupled REST API with stateless JWT authentication:

| Layer | Technology |
|---|---|
| **Framework** | Spring Boot 3.0.6 |
| **Language** | Java 17 |
| **Security** | Spring Security 6 & JJWT (JSON Web Token) |
| **Persistence** | Spring Data JPA (Hibernate) |
| **Database** | PostgreSQL |
| **Build Tool** | Apache Maven |
| **Frontend Integration** | Angular (via REST & Cross-Origin Resource Sharing / CORS) |

---

## Core Features ⚡

### 1. Authentication & Security
- **Role-Based Accounts**: Supports `CLIENT` and `COMPANY` roles.
- **Client & Company Registration**: Unique email validation and BCrypt password encryption.
- **JWT Authentication**: Stateless token-based login; token transmitted via standard `Authorization: Bearer <token>` header.
- **CORS Support**: Pre-configured `SimpleCorsFilter` enabling seamless communication with frontend clients (e.g. Angular on `http://localhost:4200`).

### 2. Client Capabilities (`/api/client`)
- **Browse Ads**: View all available services posted by verified companies.
- **Search Services**: Search ads by service name.
- **Ad Details**: Inspect detailed service descriptions, pricing, and company info.
- **Book Services**: Submit reservation requests for specific dates.
- **Booking History**: Track the status of submitted bookings (`PENDING`, `APPROVED`, `REJECTED`).
- **Submit Reviews**: Rate and review completed services.

### 3. Company Capabilities (`/api/company`)
- **Post Service Ads**: Publish service listings including service name, description, price, and image.
- **Manage Ads**: View, update, or delete existing service ads.
- **Booking Management**: View all incoming client bookings and update reservation status (`Approve` / `Reject`).

---

## API Endpoints Reference 📡

### Public & Authentication Endpoints
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/client/sign-up` | Register a new client account |
| `POST` | `/company/sign-up` | Register a new company account |
| `POST` | `/authenticate` | Login with username (email) and password; returns JWT token in header and user info in body |

### Client Endpoints (`/api/client/**` - Requires JWT)
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/client/ads` | Fetch all active service listings |
| `GET` | `/api/client/search/{name}` | Search services by name |
| `GET` | `/api/client/ad/{adId}` | Get details of a specific ad |
| `POST` | `/api/client/book-service` | Submit a booking reservation |
| `GET` | `/api/client/my-bookings/{userId}` | Get all bookings for a client |
| `POST` | `/api/client/review` | Submit a review and rating for a booking |

### Company Endpoints (`/api/company/**` - Requires JWT)
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/company/ad/{userId}` | Create a new service ad (with image upload) |
| `GET` | `/api/company/ads/{userId}` | Get all ads created by a company |
| `GET` | `/api/company/ad/{adId}` | Get details of a specific ad |
| `PUT` | `/api/company/ad/{adId}` | Update an existing ad |
| `DELETE`| `/api/company/ad/{adId}` | Delete an ad |
| `GET` | `/api/company/bookings/{companyId}` | Get all bookings submitted to this company |
| `GET` | `/api/company/booking/{bookingId}/{status}` | Change booking status (`Approve` / `Reject`) |

---

## Getting Started & Setup 🚀

### Prerequisites
Make sure the following tools are installed on your machine:
- **Java 17 (JDK)**
- **PostgreSQL 14+** (running on port `5432`)
- **Git**

---

### Step 1: Configure the Database

1. Open PostgreSQL (via pgAdmin or SQL shell / `psql`):
   ```sql
   CREATE DATABASE service_booking_system_db;
   ```

2. Configure database credentials in `Spring boot app/src/main/resources/application.properties`:
   ```properties
   spring.application.name=ServiceBookingSystem

   spring.datasource.url=jdbc:postgresql://localhost:5432/service_booking_system_db
   spring.datasource.username=postgres
   spring.datasource.password=YOUR_POSTGRES_PASSWORD

   spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
   spring.jpa.show-sql=true
   spring.jpa.hibernate.ddl-auto=update
   ```
   *(Hibernate will automatically create and update the database schema on first run).*

---

### Step 2: Set Environment Variables (if needed)

Ensure `JAVA_HOME` is pointing to your Java 17 installation:

**PowerShell (Windows):**
```powershell
$env:JAVA_HOME = "C:\Program Files\Java\jdk-17"
```

---

### Step 3: Build & Run the Backend

Navigate to the `Spring boot app` directory:
```bash
cd "Spring boot app"
```

1. **Verify build and compilation:**
   ```powershell
   .\mvnw.cmd clean test-compile
   ```

2. **Run the Spring Boot application:**
   ```powershell
   .\mvnw.cmd spring-boot:run
   ```

Once started, the backend API will be live at:
```
http://localhost:8080/
```

---

### Step 4: Connecting the Angular Application

In your Angular application (e.g. `Angular app/src/app/...`), configure the service base URL to point to the Spring Boot server:

```typescript
const BASIC_URL = 'http://localhost:8080/';
```

All requests from the Angular app (`client/sign-up`, `company/sign-up`, `authenticate`, `/api/client/**`, `/api/company/**`) will communicate directly with the Spring Boot backend via CORS.
