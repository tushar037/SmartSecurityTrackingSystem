# Smart Security Tracking System

A comprehensive Java-based security incident tracking and monitoring system for organizations to report, track, and analyze security events in real-time. The system features QR code-based staff identification, real-time attendance tracking, and automated reporting capabilities.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [Usage Examples](#usage-examples)
- [Database Schema](#database-schema)
- [Key Components](#key-components)
- [Contributing](#contributing)
- [License](#license)

## Features

- ✅ **Staff Management** - Create, retrieve, and manage staff profiles
- ✅ **QR Code Generation** - Automatic QR code generation for each staff member for secure identification
- ✅ **Attendance Tracking** - Real-time attendance check-in/check-out with timestamp tracking
- ✅ **Working Hours Monitoring** - Automatic calculation of working minutes per day
- ✅ **Excel Report Generation** - Generate detailed attendance reports in Excel format
- ✅ **Scheduled Tasks** - Automated cleanup and daily report scheduling
- ✅ **RESTful API** - Complete REST API for all operations
- ✅ **Swagger UI Integration** - Interactive API documentation
- ✅ **Cross-Origin Support** - CORS enabled for web frontend integration
- ✅ **MySQL Database** - Persistent data storage with auto-schema generation

## Tech Stack

- **Language**: Java 21
- **Framework**: Spring Boot 3.5.3
- **Build Tool**: Maven
- **Database**: MySQL
- **ORM**: JPA/Hibernate
- **API Documentation**: Springdoc OpenAPI (Swagger UI)
- **QR Code**: Google ZXing Library
- **Excel Generation**: Apache POI
- **Code Generation**: Lombok
- **Server Port**: 8080

## Project Structure

```
SecurityTrackingSystem/
├── pom.xml                                           # Maven configuration
├── src/main/
│   ├── java/edu/tushar/securitytrackingsystem/
│   │   ├── SecuritytrackingsystemApplication.java   # Main Spring Boot Application
│   │   ├── controller/                              # REST API Controllers
│   │   │   ├── StaffController.java                # Staff CRUD operations
│   │   │   └── AttendanceController.java           # Attendance operations
│   │   ├── service/                                # Business Logic Layer
│   │   │   ├── StaffService.java                   # Staff service interface
│   │   │   ├── AttendanceService.java              # Attendance service interface
│   │   │   └── Implement/
│   │   │       ├── StaffServiceImpl.java            # Staff implementation
│   │   │       └── AttendanceServiceImpl.java       # Attendance implementation
│   │   ├── repository/                             # Data Access Layer
│   │   │   ├── StaffRepository.java                # Staff database operations
│   │   │   └── AttendanceRepository.java           # Attendance database operations
│   │   ├── entity/                                 # Data Models
│   │   │   ├── Staff.java                          # Staff entity
│   │   │   ├── Attendance.java                     # Attendance entity
│   │   │   └── AttendanceDTO.java                  # Attendance data transfer object
│   │   ├── exception/                              # Custom Exceptions
│   │   ├── exceptionhandler/                       # Global Exception Handling
│   │   ├── util/                                   # Utility Classes
│   │   │   ├── QRCodeGenerator.java                # QR code generation utility
│   │   │   └── ExcelGenerator.java                 # Excel report generation
│   │   └── schedular/                              # Scheduled Tasks
│   │       └── AttendanceScheduler.java            # Automated scheduling tasks
│   └── resources/
│       └── application.properties                   # Application configuration
└── .mvn/                                            # Maven wrapper

```

## Prerequisites

Before you begin, ensure you have the following installed:

- **Java Development Kit (JDK)** - Version 21 or higher
- **Maven** - Version 3.6+ (or use the included Maven wrapper)
- **MySQL Server** - Version 5.7+ or 8.0+
- **Git** - For version control

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/tushar037/SecurityTrackingSystem.git
cd SecurityTrackingSystem
```

### 2. Prerequisites Setup

#### Install MySQL and Create Database

```bash
# Start MySQL service
mysql -u root -p

# You can create the database manually or let Spring Boot auto-create it
# The application will automatically create the database if it doesn't exist
CREATE DATABASE securitytrackingsys;
```

### 3. Build the Project

```bash
# Using Maven wrapper (recommended)
./mvnw clean install

# Or using Maven (if installed globally)
mvn clean install
```

## Configuration

### Database Configuration

Edit `src/main/resources/application.properties`:

```properties
# Application name
spring.application.name=securitytrackingsystem

# Database Configuration
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/securitytrackingsys?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=root

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Server Configuration
server.port=8080
```

**Configuration Parameters:**

| Parameter | Default | Description |
|-----------|---------|-------------|
| `spring.datasource.url` | `jdbc:mysql://localhost:3306/securitytrackingsys` | MySQL database URL |
| `spring.datasource.username` | `root` | MySQL username |
| `spring.datasource.password` | `root` | MySQL password |
| `spring.jpa.hibernate.ddl-auto` | `update` | Auto-create/update database schema |
| `server.port` | `8080` | Server port |

## Running the Application

### Method 1: Using Maven Wrapper

```bash
./mvnw spring-boot:run
```

### Method 2: Using Maven

```bash
mvn spring-boot:run
```

### Method 3: Build and Run JAR

```bash
# Build the JAR
./mvnw clean package

# Run the JAR
java -jar target/securitytrackingsystem-0.0.1-SNAPSHOT.jar
```

### Verify Application is Running

Once started, you should see output like:

```
Started SecuritytrackingsystemApplication in X.XXX seconds
```

Access the application at: `http://localhost:8080`

## API Documentation

### Swagger UI

Interactive API documentation is available at:

```
http://localhost:8080/swagger-ui.html
```

## Usage Examples

### 1. Add a New Staff Member

**Endpoint**: `POST /api/staff`

```bash
curl -X POST http://localhost:8080/api/staff \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "mobile": "9876543210",
    "email": "john.doe@example.com",
    "age": 28,
    "address": "123 Main Street",
    "designation": "Security Officer"
  }'
```

**Response**:
```json
{
  "id": 1,
  "name": "John Doe",
  "mobile": "9876543210",
  "email": "john.doe@example.com",
  "age": 28,
  "address": "123 Main Street",
  "designation": "Security Officer",
  "qrCodePath": "/qrcodes/staff_1_qr.png"
}
```

### 2. Get All Staff Members

**Endpoint**: `GET /api/staff`

```bash
curl http://localhost:8080/api/staff
```

### 3. Get Staff by ID

**Endpoint**: `GET /api/staff/{id}`

```bash
curl http://localhost:8080/api/staff/1
```

### 4. Delete Staff Member

**Endpoint**: `DELETE /api/staff/{id}`

```bash
curl -X DELETE http://localhost:8080/api/staff/1
```

### 5. Record Attendance Check-In

**Endpoint**: `POST /api/attendance/checkin`

```bash
curl -X POST http://localhost:8080/api/attendance/checkin \
  -H "Content-Type: application/json" \
  -d '{
    "staffId": 1,
    "date": "2024-09-03"
  }'
```

### 6. Record Attendance Check-Out

**Endpoint**: `POST /api/attendance/checkout`

```bash
curl -X POST http://localhost:8080/api/attendance/checkout \
  -H "Content-Type: application/json" \
  -d '{
    "staffId": 1,
    "date": "2024-09-03"
  }'
```

### 7. Get Attendance Records

**Endpoint**: `GET /api/attendance`

```bash
curl http://localhost:8080/api/attendance
```

### 8. Generate Attendance Report (Excel)

**Endpoint**: `GET /api/attendance/report`

```bash
curl http://localhost:8080/api/attendance/report -o attendance_report.xlsx
```

## Database Schema

### Staff Table

| Column | Type | Constraint | Description |
|--------|------|-----------|-------------|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique staff identifier |
| `name` | VARCHAR(255) | | Staff member name |
| `mobile` | VARCHAR(20) | | Phone number |
| `email` | VARCHAR(255) | | Email address |
| `age` | INT | | Age |
| `address` | VARCHAR(255) | | Physical address |
| `designation` | VARCHAR(100) | | Job designation/title |
| `qrCodePath` | VARCHAR(255) | | Path to generated QR code image |

### Attendance Table

| Column | Type | Constraint | Description |
|--------|------|-----------|-------------|
| `id` | BIGINT | PRIMARY KEY, AUTO_INCREMENT | Unique attendance record ID |
| `date` | DATE | | Attendance date |
| `checkInTime` | TIME | | Time of check-in |
| `checkOutTime` | TIME | | Time of check-out |
| `workingMinutes` | BIGINT | | Total working minutes (auto-calculated) |
| `status` | VARCHAR(50) | | Attendance status (Present, Absent, Late, etc.) |
| `staff_id` | BIGINT | FOREIGN KEY | Reference to Staff entity |

## Key Components

### Controllers

**StaffController** - Manages staff CRUD operations
- `POST /api/staff` - Add new staff
- `GET /api/staff` - Get all staff
- `GET /api/staff/{id}` - Get staff by ID
- `DELETE /api/staff/{id}` - Delete staff

**AttendanceController** - Manages attendance operations
- `POST /api/attendance/checkin` - Check-in attendance
- `POST /api/attendance/checkout` - Check-out attendance
- `GET /api/attendance` - Get all attendance records
- `GET /api/attendance/report` - Download Excel report

### Services

**StaffService** - Business logic for staff management
- Add/retrieve/delete staff members
- Auto-generate QR codes during staff registration

**AttendanceService** - Business logic for attendance tracking
- Record check-in/check-out times
- Calculate working hours
- Generate attendance reports

### Utilities

**QRCodeGenerator** - Generates QR codes for staff identification using Google ZXing library

**ExcelGenerator** - Creates Excel reports with attendance data using Apache POI

### Scheduler

**AttendanceScheduler** - Runs scheduled tasks like daily report generation and data cleanup

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is open source. Please check the repository for license information.

---

## Troubleshooting

### Database Connection Issues

**Error**: `Connection refused to host: 127.0.0.1:3306`

**Solution**: Ensure MySQL server is running:
```bash
# On Windows
net start MySQL80  # or your MySQL service name

# On macOS
brew services start mysql

# On Linux
sudo systemctl start mysql
```

### Port Already in Use

**Error**: `Port 8080 already in use`

**Solution**: Change the port in `application.properties`:
```properties
server.port=8081
```

### Authentication Issues

If you encounter MySQL authentication errors, verify your credentials in `application.properties` match your MySQL setup.

---

**For more information or support, please open an issue on GitHub.**

