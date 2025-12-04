# 🔑 SoulKey — Dynamic Environment Injection SDK (v1.0)

SoulKey is a plug-and-play **Java SDK** built to inject dynamic, typed configuration values into **Spring Boot** applications during startup.  
It allows developers to write environment keys in `application.properties` such as:

```
threshold=soulkey.<projectId>.THRESHOLD2
spring.datasource.url=soulkey.<projectId>.DB_URL1
```

At runtime, SoulKey resolves these keys by calling a remote environment service and replacing them with **typed values** (integer, string, decimal, boolean) inside the Spring Environment.

---

## 📁 Repositories

### 🔹 SoulKey SDK  
Java library that reads SDK keys, calls the remote environment service, handles type conversion, and injects resolved values into Spring Boot.

👉 **SDK Repository:**  
https://github.com/Vinith11/soulkey-sdk

---

### 🔹 SoulKey Web App  
A full stack web app that helps you with connecting to database and creating projects. Also allows you to add env so that sdk can access it.

👉 **SDK Repository:**  
https://github.com/Vinith11/soulkey-sdk

---

### 🔹 SoulKey Example App (UI + Environment Creator)  
A demo Spring Boot application that uses the SoulKey SDK, includes UI for creating environments, and exposes APIs consumed by the SDK.

👉 **Example App Repository:**  
https://github.com/Vinith11/soukey-examlpe

---

## 🚀 Features

- Dynamic property resolution via `EnvironmentPostProcessor`
- Supports **string**, **integer**, **decimal**, **boolean**
- Works seamlessly with Spring Boot’s property binding
- Zero code change for end users — only update `application.properties`

---

## 🧠 How SoulKey Works

### 1. Developer writes keys in `application.properties`
```properties
threshold=soulkey.08a10664-aadd-440f-9836-612dc8ac397a.THRESHOLD2
rate=soulkey.08a10664-aadd-440f-9836-612dc8ac397a.RATE
feature_flag=soulkey.08a10664-aadd-440f-9836-612dc8ac397a.FEATURE_FLAG
```

### 2. SoulKey Environment Post Processor runs before Spring initializes beans  
It detects all properties starting with `soulkey.` and extracts:

- `projectId`
- `envKey`

Example:  
`soulkey.08a10664-aadd-440f-9836-612dc8ac397a.RATE`

→ calls:

```
GET /api/projects/08a10664-aadd-440f-9836-612dc8ac397a/env/RATE
```

### 3. Environment service responds with typed data
```json
{
  "key": "RATE",
  "value": 12.345,
  "type": "decimal"
}
```

### 4. SDK converts the value to its proper Java type  
- decimal → `BigDecimal`  
- integer → `Long`  
- boolean → `Boolean`  
- string → `String`

### 5. SDK injects typed values back into Spring's Environment  
Compatible with:

- `@Value`
- `Environment.getProperty(...)`
- `@ConfigurationProperties`

---

## 🧪 Testing with Docker (Dual MySQL Setup)

SoulKey was validated via a custom test environment using Docker Compose:

- **mysql1** on port **3307**
- **mysql2** on port **3308**
- Same schema, different data
- Validates dynamic DB switching using SoulKey keys

Example:
```
spring.datasource.url=soulkey.<projectId>.DB_URL1
```

or

```
spring.datasource.url=soulkey.<projectId>.DB_URL2
```

---

## 📦 Tech Stack

### SoulKey SDK
- Java 17+
- Spring Boot EnvironmentPostProcessor
- Jackson for JSON parsing
- HttpClient for remote calls

### SoulKey WebApp
- Spring Boot
- React
- PostgreSQL

### Example App
- Spring Boot
- JPA/Hibernate
- MySQL
- Docker Compose
- REST API for fetching environment variables

---

## 📚 Version
**SoulKey SDK v1.0 — Stable Release**

---

## 💬 Contributing
PRs are welcome!  
If you find a bug or want a new feature, feel free to open an issue.

---

## 📝 License
MIT License
You are free to use, modify, and distribute.
