# Running the Backend

There are two common ways to start this Spring Boot backend.

## Development Mode

Use this while building and testing locally:

```bash
./mvnw spring-boot:run
```

This runs the app directly from the project source. It is the best option for normal local development.

Stop it with:

```bash
Ctrl+C
```

## Packaged JAR Mode

Use this when you want to build the final application file and run that packaged version:

```bash
./mvnw package
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

This first creates a standalone JAR file in `target/`, then runs that JAR. This is closer to how the app would be run in deployment.

## Which One Should I Use?

If your backend is already running with:

```bash
./mvnw spring-boot:run
```

you do not need to also run:

```bash
./mvnw package
java -jar target/demo-0.0.1-SNAPSHOT.jar
```

Use one or the other. For normal local development, use:

```bash
./mvnw spring-boot:run
```
