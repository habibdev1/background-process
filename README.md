# Spring Boot Background Process Application

This project demonstrates a Spring Boot application featuring a REST API for employee management and several scheduled background processes for report generation and email notifications.

## Features

*   **RESTful API:** Provides endpoints for performing CRUD (Create, Read, Update, Delete) operations on employee records.
*   **Scheduled Background Processes:**
    *   **Report Generation:** Automatically generates text, PDF, and Word documents at fixed intervals.
    *   **Email Notifications:** Sends scheduled email updates.
*   **Data Persistence:** Integrates with a database using Spring Data JPA for storing employee information.

## Technologies Used

*   **Spring Boot:** Framework for building the application.
*   **Spring Data JPA:** For database interaction and object-relational mapping.
*   **Maven:** Dependency management and build automation.
*   **iText:** Library for generating PDF documents.
*   **Apache POI:** Library for generating Microsoft Word documents.
*   **Jakarta Mail:** For sending email notifications.
*   **H2 Database (or your configured database):** In-memory or persistent database for demonstration.

## Project Structure

```
.
├───src/main/java/com/example/background_process/
│   ├───BackgroundProcessApplication.java
│   ├───ServletInitializer.java
│   ├───controller/
│   │   └───EmployeesController.java
│   ├───file/
│   │   ├───FileService.java
│   │   ├───PdfFileService.java
│   │   └───WordFileService.java
│   ├───mail/
│   │   └───Email.java
│   ├───model/
│   │   └───EmployeeModel.java
│   ├───payload/
│   │   └───APIResponse.java
│   ├───repository/
│   │   └───EmployeeRepository.java
│   └───service/
│       ├───EmailService.java
│       └───ReportService.java
├───src/main/resources/
│   ├───application.properties  (or application.yml)
│   └───...
└───pom.xml
```

## Setup and Running the Application

### Prerequisites

*   Java 17 or higher
*   Maven

### Configuration

1.  **Database Configuration:**
    By default, Spring Boot might use an in-memory H2 database. If you wish to use a persistent database (e.g., MySQL), configure your `src/main/resources/application.properties` (or `application.yml`) accordingly.
    
    Example for MySQL:
    ```properties
    spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
    spring.datasource.username=your_username
    spring.datasource.password=your_password
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
    ```

2.  **Email Configuration:**
    The `mail/Email.java` class currently contains hardcoded SMTP credentials. **For production use, it is highly recommended to externalize these credentials using environment variables or Spring Boot's configuration properties to avoid exposing sensitive information.**
    
    You might need to adjust `mail.ahsanhabib.space` and `mail@ahsanhabib.space` to your SMTP server and sender email.

3.  **File Storage Path:**
    Generated reports (text, PDF, Word) are saved to `/var/public/uploads/`. Ensure this directory exists and the application has write permissions to it. You might want to configure this path in `application.properties` for flexibility.

### Build and Run

1.  **Clone the repository:**
    ```bash
    git clone git@github.com:habibdev1/background-process.git
    cd springbootbackgroundprocess
    ```

2.  **Build the project:**
    ```bash
    mvn clean install
    ```

3.  **Run the application:**
    ```bash
    java -jar target/springbootbackgroundprocess-0.0.1-SNAPSHOT.jar
    ```
    Alternatively, you can run it directly from your IDE.

## API Endpoints

The REST API is available under the `/api` base path.

### Employee Management (`/api/employee` or `/api/employees`)

*   **Create Employee:**
    *   `POST /api/employee`
    *   **Body:** `application/json`
    *   Example: `{"name": "John Doe", "email": "john.doe@example.com", "mobile": "1234567890"}`
    *   **Response:** `200 OK` (with the created employee object)

*   **Get All Employees:**
    *   `GET /api/employees`
    *   **Response:** `200 OK` (list of all employees)

*   **Get Employee by ID:**
    *   `GET /api/employees/{id}`
    *   **Response:** `200 OK` (employee object) or `404 Not Found`

*   **Update Employee:**
    *   `PUT /api/employee/{id}`
    *   **Body:** `application/json`
    *   Example: `{"name": "Jane Doe", "email": "jane.doe@example.com", "mobile": "0987654321"}`
    *   **Response:** `200 OK` or `404 Not Found`

*   **Delete Employee:**
    *   `DELETE /api/employee/{id}`
    *   **Response:** `200 OK` or `404 Not Found`

## Background Processes

The application includes several scheduled tasks:

*   **General Report:** A placeholder task in `ReportService` that runs every 100 minutes.
*   **Text File Generation:** `FileService` generates `report_timestamp.txt` in `/var/public/uploads/` every 100 minutes.
*   **PDF File Generation:** `PdfFileService` generates `report_timestamp.pdf` in `/var/public/uploads/` every 100 minutes.
*   **Word File Generation:** `WordFileService` generates `report_timestamp.docx` in `/var/public/uploads/` every 100 minutes.
*   **Email Sending:** `EmailService` sends an email every day at 11:59 AM (server time).

## Important Security Note

**Hardcoded Credentials:** The `mail/Email.java` file contains hardcoded SMTP username and password. This is a significant security vulnerability. In a production environment, these should *always* be externalized using environment variables, a secrets management service, or Spring Boot's externalized configuration properties.

## Contributing

Feel free to fork the repository, make improvements, and submit pull requests.

## License
