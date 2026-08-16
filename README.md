# Spring Boot Hello World

A simple Spring Boot REST API application demonstrating basic setup and a "Hello World" endpoint.

## Overview

This is a minimal Spring Boot application that provides a single REST endpoint returning "Hello World". It serves as a foundation for learning Spring Boot fundamentals or as a starting point for more complex applications.

## Prerequisites

- **Java Development Kit (JDK)**: Version 21 or higher
- **Apache Maven**: Version 3.6 or higher (for building the project)
- **Docker**: For containerized deployment (optional)
- **Git**: For version control (optional)

## Project Structure

```
spring-boot-hello-world/
├── src/
│   ├── main/
│   │   ├── java/com/example/demo/
│   │   │   ├── DemoApplication.java       # Spring Boot application entry point
│   │   │   └── DemoController.java        # REST controller with endpoints
│   │   └── resources/
│   │       ├── application.properties     # Application configuration
│   │       ├── static/                    # Static resources (CSS, JS, images)
│   │       └── templates/                 # HTML templates
│   └── test/
│       └── java/com/example/demo/
│           └── DemoApplicationTests.java  # Unit tests
├── pom.xml                                # Maven project configuration
├── Dockerfile                             # Docker image configuration
├── .dockerignore                          # Docker build context exclusions
└── README.md                              # This file
```

## Technologies Used

- **Spring Boot 4.1.0**: Web framework for building REST APIs
- **Java 21**: Programming language
- **Maven**: Build and dependency management
- **Lombok**: (Optional) Library for reducing boilerplate code
- **Spring Boot Starter WebMVC**: For building web and REST applications
- **Docker**: Containerization for consistent deployment across environments

## Building the Project

### Using Maven

1. Navigate to the project directory:
   ```bash
   cd spring-boot-hello-world
   ```

2. Build the project:
   ```bash
   mvn clean package
   ```

   This command will:
   - Clean any previous builds (`target/` directory)
   - Compile the source code
   - Run tests
   - Package the application as a JAR file

3. The built JAR file will be located at: `target/demo-0.0.1-SNAPSHOT.jar`

## Running the Application

### From Maven

```bash
mvn spring-boot:run
```

### From Built JAR

```bash
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

### From IDE

Run `DemoApplication.java` as a Java Application in your IDE (VS Code, IntelliJ IDEA, Eclipse, etc.)

The application will start on the default port: **http://localhost:8080**

## Docker Deployment

### Building the Docker Image

From the project root directory:

```bash
docker build -t spring-boot-hello-world:latest .
```

This uses a multi-stage build process:
1. **Build stage**: Compiles the application using Maven
2. **Runtime stage**: Runs the application using a lightweight JRE Alpine image

### Running the Docker Container

```bash
docker run -p 8080:8080 spring-boot-hello-world:latest
```

### Verification

Test the running container:

```bash
curl http://localhost:8080/hello
```

### Docker Compose (Optional)

For easier management, create a `docker-compose.yml`:

```yaml
version: '3.8'

services:
  demo-app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_APPLICATION_NAME=demo
    restart: unless-stopped
```

Then run with:

```bash
docker-compose up
```

## API Endpoints

### Hello World Endpoint

- **URL**: `/hello`
- **Method**: `GET`
- **Response**: `Hello World !!`
- **Example**:
  ```bash
  curl http://localhost:8080/hello
  ```

## Configuration

Application settings are defined in `src/main/resources/application.properties`:

- `spring.application.name=demo`: Sets the application name

You can modify this file to change application behavior, such as:
- Server port: `server.port=8080`
- Logging level: `logging.level.root=INFO`
- Custom application properties

## Testing

Run unit tests using Maven:

```bash
mvn test
```

Tests are located in `src/test/java/` and use Spring Boot Test framework.

## Development

### Adding New Endpoints

1. Open `src/main/java/com/example/demo/DemoController.java`
2. Add new methods with `@GetMapping`, `@PostMapping`, etc. annotations
3. Rebuild and run the application

### Example:

```java
@PostMapping("echo")
public String echo(@RequestParam String message) {
    return "Echo: " + message;
}
```

## Troubleshooting

- **Port 8080 already in use**: Change the port in `application.properties`:
  ```properties
  server.port=8081
  ```

- **Java version mismatch**: Ensure you have Java 21 installed:
  ```bash
  java -version
  ```

- **Maven not found**: Ensure Maven is installed and added to your PATH

## Next Steps

- Deploy the application using Docker and Docker Compose
- Set up Kubernetes manifests for production deployments
- Explore Spring Data JPA for database integration
- Add a database layer (H2, MySQL, PostgreSQL)
- Implement service layer with business logic
- Add request/response validation with annotations
- Configure Spring Security for authentication and authorization
- Add logging using SLF4J and Logback

## License

This project is provided as-is for educational and development purposes.

## Support

For issues or questions, refer to the [Spring Boot Documentation](https://spring.io/projects/spring-boot).
