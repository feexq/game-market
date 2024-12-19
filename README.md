# Game Market

Game Market is a **Steam-inspired backend service** that manages the complete lifecycle of digital game sales — from intuitive catalog browsing to order placement and automated license-key activation **(external service mock)**. Built with **Spring Boot 3**, the system features **AOP-driven feature toggles**, ensuring scalability through **Liquibase** migrations and **Testcontainers**-backed reliability.

![Java 17](https://img.shields.io/badge/Java-17-orange?logo=openjdk)
![Spring Boot 3.3](https://img.shields.io/badge/Spring%20Boot-3.3-6DB33F?logo=springboot)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql)
![Docker](https://img.shields.io/badge/Docker-compose-2496ED?logo=docker)

## ⭐ Key Features

- **Dynamic AOP Toggles** — Real-time control over endpoints (e.g., `/sale`) via custom annotations.
- **GitHub Auth** — Integrated OAuth 2.0 flow with JWT-based protection **(no local passwords)**.
- **Automated Key Activation** — Seamless integration with downstream services **(WireMock stubs)**.
- **Catalog Lifecycle** — Managed products, customers, and orders with automated schema evolution.
- **Production-Ready Infra** — Full Docker environment with PostgreSQL, WireMock, and JSON logging.

## 🚀 Quick Start

1. **Spin up Infrastructure**:
   ```bash
   docker-compose up -d && docker-compose -f scripts/compose.yaml up -d
   ```
2. **Run Application**:
   ```bash
   ./gradlew bootRun -x test
   ```

*Swagger UI: `http://localhost:8080/swagger-ui.html`*

> [!NOTE]
> **Key Activation Mock:** The key activation microservice is mocked using WireMock. To successfully test order placement and key activation, you must use a game title/key defined in the `stubs.json` file.

## ⚙️ Configuration
<details>
<summary><b>Required environment variables</b></summary>

| Variable | Description | Default (Docker) |
|----------|-------------|------------------|
| `DB_HOST` | Database host | `localhost` |
| `DB_PORT` | Database port | `5555` |
| `DB_NAME` | Database name | `mydatabase` |
| `DB_USER` | Database user | `user` |
| `DB_PASSWORD` | Database password | `password` |
| `KEY_SERVICE_URL` | Mock key service URL | `http://localhost:8081` |
| `GITHUB_CLIENT_ID` | GitHub OAuth2 Client ID | `your_id` |
| `GITHUB_CLIENT_SECRET`| GitHub OAuth2 Client Secret | `your_secret` |
| `APP_URL` | Application base URL | `http://localhost:8080` |
| `GITHUB_URL` | GitHub Base URL | `https://github.com/` |
| `GITHUB_API_URL` | GitHub API URL | `https://api.github.com/` |

</details>

## 🛠 Tech Stack
<details>
<summary><b>View detailed technology layers</b></summary>

| Category | Technologies |
|----------|--------------|
| **Core** | Java 17, Spring Boot 3.3, Security, Data JPA, Spring AOP, MapStruct |
| **Data** | PostgreSQL 15, Liquibase 4.30, HikariCP |
| **Test** | JUnit 5, Testcontainers, WireMock, AssertJ, JaCoCo |
| **Ops**  | Docker Compose, Gradle 8.x, Logstash (JSON logging) |

</details>

## 🔌 API Examples
<details>
<summary><b>View common curl commands</b></summary>

All endpoints are available without authentication for demo purposes.

### Create Product
```bash
curl -X POST http://localhost:8080/api/v1/products \
  -H "Content-Type: application/json" \
  -d '{ "title": "Hades", "price": 24.99, "developer": "Supergiant Games" }'
```

### Place Order
```bash
curl -X POST http://localhost:8080/api/v1/orders/cust-ref/cart-id \
  -H "Authorization: Bearer <token>" \
  -d '{ "entries": [{ "gameType": "Hades", "quantity": 1 }], "total": 24.99 }'
```
</details>
