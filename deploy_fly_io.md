# Deploy Spring Boot 4 (Java 25) on Fly.io

This guide explains how to deploy a Spring Boot 4.x application using Java 25 on Fly.io using a Dockerfile.

---

## 1. Prerequisites

- Java 25 installed  
- Maven Wrapper (`./mvnw`)  
- Docker installed  
- Fly CLI installed  
- Logged in to Fly.io:
```
fly auth login
```

---

## 2. Build the Spring Boot JAR

```
./mvnw clean package -DskipTests
```

Your JAR should appear in:

```
target/*.jar
```

---

## 3. Dockerfile (Java 25)

Create a file named `Dockerfile`:

```
FROM eclipse-temurin:25-jdk
WORKDIR /app
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## 4. Initialize Fly.io

```
fly launch
```

Choose:

- No Postgres  
- No Redis  
- Yes to create `fly.toml`  
- No to generating a Dockerfile (you already have one)

---

## 5. Deploy the application

```
fly deploy
```

Fly will build the Docker image remotely and deploy it.

Your app will be available at:

```
https://<app-name>.fly.dev
```

---

## 6. Stop the app (scale to zero)

```
fly scale count 0
```

---

## 7. Restart the app

```
fly scale count 1
```

---

## 8. Delete the app (to ensure $0.00 cost)

```
fly apps destroy <app-name>
```

---

## Project Structure Example

```
spring-boot-application/
 ├─ src/
 ├─ target/
 ├─ Dockerfile
 ├─ fly.toml
 ├─ pom.xml
 └─ README.md
```

---

## Notes

- Buildpacks do not support Java 25 yet → Dockerfile required  
- Ensure `target/` exists before running `fly deploy`  
- Ensure `.dockerignore` does NOT exclude `target/`  
