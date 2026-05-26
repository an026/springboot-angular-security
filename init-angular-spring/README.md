# Setting Up Spring Boot with Angular

## Stack

* Spring Boot 4.x
* Angular 20+
* Node 22+
* Maven
* npm

---

# 1. Create Spring Boot Backend

Use Spring Initializr.

Dependencies:

* Spring Web
* Spring Data JPA (optional)
* Spring Security (optional)

Example controller:

```java
@RestController
@RequestMapping("/api")
class HelloController {

    @GetMapping("/hello")
    Map<String, String> hello() {
        return Map.of("message", "Hello from Spring Boot");
    }
}
```

Run backend:

```bash
./mvnw spring-boot:run
```

Backend runs on:

```txt
localhost:8080
```

---

# 2. Create Angular Frontend

Inside the project root:

```bash
ng new frontend
```

Then:

```bash
cd frontend
npm install
npm start
```

Frontend runs on:

```txt
localhost:4200
```

---

# 3. Configure Angular Proxy

Create:

```txt
frontend/proxy.conf.json
```

```json
{
  "/api": {
    "target": "http://localhost:8080",
    "secure": false
  }
}
```

Update `frontend/package.json`:

```json
{
  "scripts": {
    "start": "ng serve --proxy-config proxy.conf.json"
  }
}
```

Now Angular can call:

```ts
fetch("/api/hello")
```

instead of hardcoding the backend URL.

---

# 4. Development Workflow

Run frontend and backend separately.

Terminal 1:

```bash
./mvnw spring-boot:run
```

Terminal 2:

```bash
cd frontend
npm start
```

This is the modern standard workflow.

---

# 5. Production Build Concept

Angular builds static files:

```bash
npm run build
```

Output:

```txt
frontend/dist/
```

Spring Boot serves static files from:

```txt
src/main/resources/static
```

Production flow:

```txt
Angular build
↓
Copy dist files into Spring static folder
↓
Spring Boot serves frontend + API
```

Result:

```txt
localhost:8080/
localhost:8080/api/*
```

---

# 6. Optional Maven + npm Integration

Only needed if you want:

```bash
./mvnw package
```

to build both backend and frontend automatically.

Example `pom.xml` plugin:

```xml
<plugin>
  <groupId>com.github.eirslett</groupId>
  <artifactId>frontend-maven-plugin</artifactId>
  <version>1.15.1</version>

  <configuration>
    <workingDirectory>frontend</workingDirectory>
    <nodeVersion>v22.12.0</nodeVersion>
  </configuration>

  <executions>

    <execution>
      <id>install-node-and-npm</id>
      <goals>
        <goal>install-node-and-npm</goal>
      </goals>
    </execution>

    <execution>
      <id>npm-install</id>
      <goals>
        <goal>npm</goal>
      </goals>
      <configuration>
        <arguments>install</arguments>
      </configuration>
    </execution>

    <execution>
      <id>npm-build</id>
      <goals>
        <goal>npm</goal>
      </goals>
      <configuration>
        <arguments>run build</arguments>
      </configuration>
    </execution>

  </executions>
</plugin>
```

---

# Key Modern Concept

Development:

```txt
Angular dev server
+
Spring Boot API server
```

Production:

```txt
Angular static build
+
Spring Boot serving static files + API
```
