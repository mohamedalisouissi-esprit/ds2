# Simple Spring Boot Application

A minimalistic Spring Boot application with Docker support.

## Features

- Simple REST API endpoint
- Health check endpoint
- Spring Boot Actuator for monitoring
- Docker containerization with multi-stage build

## Getting Started

### Prerequisites

- Java 17 or higher
- Maven 3.6+
- Docker (for containerization)

### Running Locally

```bash
# Build the application
mvn clean package

# Run the application
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

### Docker Build and Run

```bash
# Build the Docker image
docker build -t spring-boot-demo .

# Run the Docker container
docker run -p 8080:8080 spring-boot-demo
```

## API Endpoints

- `GET /api/`: Welcome message
- `GET /api/health`: Health check
- `GET /actuator/health`: Spring Boot Actuator health endpoint

## Environment Variables

The application can be configured using the following environment variables:

- `SERVER_PORT`: The port the application will run on (default: 8080)
- `JAVA_OPTS`: JVM options (default: "-Xms256m -Xmx512m")