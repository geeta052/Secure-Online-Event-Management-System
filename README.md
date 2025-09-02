# 🛡️ Secure Online Event Management System (SOEMS)
Event Management System

## 📌 Overview
SOEMS (Secure Online Event Management System) is a **RESTful API** built for managing users, events, registrations, and payments.  
It supports both **USER** and **ADMIN** roles with **JWT-based authentication** and integrates with **Razorpay** for online payments.

---

## 🚀 Features
- **User Authentication & Authorization** (JWT-based login, signup, forgot/reset password)
- **Role-based Access Control** (USER / ADMIN)
- **Event Management** (Create, update, delete events – Admin only)
- **Event Registration** (Users can register & cancel participation)
- **Payments Module** (User payments & Admin revenue reports)
- **Razorpay Payment Gateway Integration**
- **Search & Filters** for events (location, amount, startDate, eventName)
- **Upcoming Events & Seat Availability**

---

## ⚙️ Technical Stack
- **Backend:** Spring MVC (Spring 5), Hibernate ORM
- **Database:** PostgreSQL
- **Security:** JWT Authentication, BCrypt Password Hashing
- **Frontend:** JSP + AJAX (asynchronous REST API calls)
- **Build Tool:** Maven
- **Architecture:** Layered (Controller → Service → DAO → Entity)
---
## 🗂️ ER Diagram

### **Entities**
- **User** → `userId, username, email, password, role, resetToken`
- **Event** → `eventId, eventName, description, location, startDate, endDate, registrationDeadline, maxParticipants, participantsCount, amount, createdBy`
- **Registration** → `registrationId, userId, eventId, registeredAt`
- **Payment** → `paymentId, registrationId, userId, eventId, amount, status, paymentDate`

### **Relationships**
- A **User** can register for many **Events**
- An **Event** can have many **Registrations**
- Each **Registration** generates one **Payment**

---

## 📂 API Endpoints

### 🔐 Authentication
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST   | `/auth/register` | Register a new user (USER/ADMIN) | No |
| POST   | `/auth/login` | Login user, returns JWT | No |
| GET    | `/auth/logout` | Logout and clear JWT | Yes |
| POST   | `/forgot-password` | Request reset link | No |
| GET    | `/reset-password?token=...` | Open password reset form | No |
| POST   | `/reset-password` | Reset password | No |

---

### 👤 User Management
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET    | `/users/{id}` | Get user by ID | Yes |
| GET    | `/users` | Get all users | Yes (ADMIN) |
| POST   | `/users/update` | Update user details | Yes |
| DELETE | `/users/{id}` | Delete user | Yes (ADMIN) |

---

### 🎟️ Event Management
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST   | `/events` | Create new event | Yes (ADMIN) |
| PUT    | `/events/{id}` | Update event | Yes (ADMIN) |
| DELETE | `/events/{id}` | Delete event | Yes (ADMIN) |
| GET    | `/events` | Get all events | No |
| GET    | `/events/{id}` | Get event by ID | No |
| GET    | `/events/upcoming` | Get upcoming events | No |
| GET    | `/events/search` | Search events by filters | No |
| GET    | `/events/{eventId}/available-seats` | Get available seats | No |
| GET    | `/events/my-events` | Get admin’s own events | Yes (ADMIN) |

---

### 📝 Registrations
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST   | `/registrations/event/{eventId}/user/{userId}` | Register user for event | Yes |
| GET    | `/registrations/user/{userId}` | Get user’s registrations | Yes |
| DELETE | `/registrations/cancel/{eventId}/{userId}` | Cancel own registration | Yes |
| GET    | `/registrations` | Get all registrations | Yes (ADMIN) |
| GET    | `/registrations/{id}` | Get registration by ID | Yes (ADMIN) |
| PUT    | `/registrations/{id}` | Update registration | Yes (ADMIN) |
| DELETE | `/registrations/{id}` | Delete registration | Yes (ADMIN) |
| GET    | `/registrations/event/{eventId}` | Get registrations for event | Yes (ADMIN) |

---

### 💳 Payments
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST   | `/api/payments` | Create new payment | Yes |
| GET    | `/api/payments/{id}` | Get payment by ID | Yes |
| GET    | `/api/payments/registration/{registrationId}` | Get payment by registration | Yes |
| GET    | `/api/payments/user/{userId}` | Get user’s payments | Yes |
| GET    | `/api/payments/event/{eventId}` | Get event payments | Yes (ADMIN) |
| GET    | `/api/payments/event/{eventId}/revenue` | Get event revenue | Yes (ADMIN) |
| POST   | `/api/payments/create-order?amount=1500` | Create Razorpay order | Yes |
| POST   | `/api/payments/verify` | Verify Razorpay payment | Yes |

---

## 🛠️ Setup Instructions
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/soems.git
   cd soems
   ```
2. Configure **application.properties** with your DB and Razorpay keys:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/soems
   spring.datasource.username=root
   spring.datasource.password=yourpassword

   razorpay.key.id=your_key
   razorpay.key.secret=your_secret
   ```
3. Build & run with Maven:
   ```bash
   mvn clean install
   mvn spring-boot:run
   ```
4. Access API at:
   ```
   http://localhost:8080/EventManagement
   ```

---

## 📖 Example Requests

### Register User
```json
POST /auth/register
{
  "username": "Diganta",
  "email": "diganta@gmail.com",
  "password": "Diganta@123",
  "role": "ADMIN"
}
```

### Create Event (Admin)
```json
POST /events
{
  "eventName": "Annual Tech Conference",
  "description": "A conference on latest technology trends.",
  "location": "Bangalore",
  "startDate": "2025-12-12",
  "endDate": "2025-12-15",
  "registrationDeadline": "2025-12-01",
  "maxParticipants": 500,
  "amount": 1500,
  "createdBy": 2
}
```

### Payment Verification
```json
POST /api/payments/verify
{
  "orderId": "order_9A33XWu170gUtm",
  "paymentId": "pay_29QQoUBi66xm2f",
  "signature": "abcdefg"
}
```

---
