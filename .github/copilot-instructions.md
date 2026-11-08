# Copilot Instructions for Spring Boot Hello World Health

## Quick Reference

**Project Type**: Java Spring Boot REST API (Spring Boot 4.1.2, Java 21)  
**Package Manager**: Maven  
**IDE Integration**: VS Code with Spring Boot Extension  
**Port**: 8080 (default)

## Build & Compile

```bash
# Full clean build with tests
mvn clean package

# Quick compile without tests
mvn clean compile

# Compile only tests
mvn test-compile
```

## Running & Testing

### Run the Application

```bash
# Via Spring Boot Maven plugin
mvn spring-boot:run

# Via compiled JAR
java -jar target/demo-0.0.1-SNAPSHOT.jar

# Via IDE: Run `DemoApplication.java` as a Java Application
```

### Run Tests

```bash
# Run all tests
mvn test

# Run a specific test class
mvn test -Dtest=DemoApplicationTests

# Run a specific test method
mvn test -Dtest=DemoApplicationTests#contextLoads
```

### Verify Application

Once running, test the endpoint:

```bash
curl http://localhost:8080/hello
```

Expected response: `Hello World !!`

## Project Structure

```
src/
├── main/
│   ├── java/com/example/demo/
│   │   ├── DemoApplication.java      # Spring Boot entry point with @SpringBootApplication
│   │   └── DemoController.java       # REST controller defining endpoints
│   └── resources/
│       └── application.properties     # Spring configuration
└── test/
    └── java/com/example/demo/
        └── DemoApplicationTests.java  # Spring Boot integration tests
```

## Key Architectural Patterns

### Spring Boot Structure

- **@SpringBootApplication**: Single entry point (DemoApplication.java) that enables auto-configuration and component scanning for the `com.example.demo` package
- **@RestController**: Controllers are REST controllers by default; methods return response bodies directly
- **@GetMapping**: Simple endpoint decoration for HTTP GET methods
- **Embedded Tomcat**: Application runs with embedded servlet container on port 8080

### No Persistence Layer

This is intentionally minimal—there is no database, service layer, or repositories. The controller directly returns data.

### No Security

Spring Security is not configured. The `/hello` endpoint is public and requires no authentication.

## Dependencies & Key Libraries

- **spring-boot-starter-webmvc**: Web and REST framework (includes Spring Web, Servlet API, embedded Tomcat)
- **spring-boot-starter-webmvc-test**: Spring Boot Test + JUnit 5 + AssertJ for integration testing
- **lombok**: Annotation processor for reducing boilerplate (configured in maven-compiler-plugin); use @Getter, @Setter, @ToString, etc. if expanding the project
- **Spring Boot Parent POM**: Manages dependency versions; no need to specify versions for spring-boot-\* or lombok

## Code Conventions

### Package Naming

- Main code: `com.example.demo.*`
- Test code: Same package structure in `src/test/java/`

### Controller Pattern

Controllers use Spring Web annotations:

```java
@RestController
public class DemoController {
    @GetMapping("hello")
    public String hello() {
        return "Hello World !!";
    }
}
```

- No trailing slashes on paths
- Return types are automatically serialized to JSON/text
- HTTP status 200 is assumed unless overridden with `@ResponseStatus` or `ResponseEntity`

### Testing

Tests extend `@SpringBootTest` which:

- Starts a full application context
- Includes all `@Component`, `@Service`, `@Repository`, `@Controller` beans
- Ideal for integration tests, not unit tests

For controller tests, consider adding `@AutoConfigureMockMvc` and `MockMvc` if testing endpoints directly.

## Lombok Configuration

Annotation processing is explicitly configured in the `maven-compiler-plugin`:

```xml
<annotationProcessorPaths>
    <path>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </path>
</annotationProcessorPaths>
```

This applies to both `compile` and `testCompile` phases. When adding Lombok annotations (@Getter, @Builder, etc.), they will be processed during compilation.

## Docker Deployment

### Build Image

```bash
docker build -t spring-boot-hello-world:latest .
```

**Multi-stage build strategy**:
1. Build stage: Maven compiles and packages the application
2. Runtime stage: Lightweight Alpine JRE runs the compiled JAR

### Run Container

```bash
docker run -p 8080:8080 spring-boot-hello-world:latest
```

### Verify Container

```bash
curl http://localhost:8080/hello
```

## Application Configuration

All Spring configuration is in `src/main/resources/application.properties`:

```properties
spring.application.name=demo
```

To customize:

- **Server port**: Add `server.port=<port>`
- **Logging level**: Add `logging.level.root=INFO`
- **Context path**: Add `server.servlet.context-path=/api`

## Common Development Tasks

### Adding a New Endpoint

1. Open [DemoController.java](../src/main/java/com/example/demo/DemoController.java)
2. Add a new method with `@GetMapping`, `@PostMapping`, etc.
3. Spring automatically binds method parameters:
   - `@RequestParam`: Query parameters
   - `@PathVariable`: URL path segments
   - Request body is auto-deserialized (if you use appropriate annotations)
4. Rebuild and test

### Adding Dependencies

1. Edit `pom.xml` under `<dependencies>`
2. Run `mvn clean compile` to download and build
3. Dependencies from Spring Boot starter parent use curated versions

Example: Adding Spring Data JPA

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
```

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Port 8080 already in use | Add `server.port=8081` to `application.properties` |
| Java version mismatch | Run `java -version`; project requires Java 21+ |
| Maven not found | Ensure Maven 3.6+ is installed and in PATH |
| Build fails on Windows | Use forward slashes in paths or Maven handles them automatically |
| Lombok annotations not working | Run `mvn clean compile`; IDE may need to be restarted |

## VS Code Integration

For optimal experience:

- **Extension Pack for Java** (Microsoft)
- **Spring Boot Extension Pack** (Pivotal/VMware)
- **Maven for Java** (Microsoft)

Build and test commands run via the Maven extension or terminal. Debugging is available through the Debugger for Java extension.

## Static Resources & Templates

- **Static files** (CSS, JS, images): `src/main/resources/static/`
- **Thymeleaf templates**: `src/main/resources/templates/`

Note: Spring WebMVC is configured; if you add templates, use `@GetMapping` + `ModelAndView` or template engine annotation processors.
