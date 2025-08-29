## Introduction

Prolog
- before we start if there are person whose name is Bob, wanna say thank you thanks for inspiration. without you we dont have this presentation. thank you and please nothing personal my friend nothing personal

Team intro 
- Bod
	- the story of our team bass start with a person whose name is … Bob Bob recently realized that he had junk food a lot in last days. He felt uncomfortable with it and want to change his diet. also he is expecting some athletic events like marathon cycling and swimming so he want to serve himself better. all he need is extra motivation to keep his diet. this is where BASS comes in.
- what the project is about
	- eCommerce for healthy meal ordering service
- problem to solve
	- keep motivation of  healthy diet up for members using streak count with achievement and coupon system
- how to solve
	- gamification concept reference from the global language learning service Duolingo
	- provide definition of ...
		- freedom-day & normal-day
		- streak count
- purpose of this project
	- apply everything we've learned through hero tech course and utilize every resource that we've provided
	- promote and advertise our team and each team members to audience 
- constraints
	- time
		- 20 days to complete the mission
	- location
		- server is located in south korea region
		- our location is Berlin, Germany
	- knowledge
		- we've just learned about Kotlin & Spring Framework
	- communication
		- new beginning with new team members

---

## 🛠️ Tech Stack

| Layer            | Tech/Tool                        |
| ---------------- | -------------------------------- |
| Language         | Kotlin                           |
| Framework        | Spring Boot, Spring Data JPA     |
| Database         | H2 (Dev), PostgreSQL (Prod)      |
| Migration        | Liquibase                        |
| Testing          | JUnit5, MockK                    |
| CI/CD            | GitHub Actions, AWS CodePipeline |
| Deployment       | AWS EC2, AWS RDS                 |
| Containerization | Docker                           |
| Docs             | SpringDoc + Swagger-UI           |
| Build Tool       | Gradle                           |

## 🧠 Mentionable Design Decisions & Trade-offs

### 1. **Why Liquibase + PostgreSQL**

**Decision:**  
We chose **Liquibase** as our database migration tool and **PostgreSQL** as our relational database.

**Rationale & Trade-offs:**

- **Liquibase**
    - ✅ **Version-controlled schema changes** — Liquibase enables us to manage database schema changes as part of the source code, promoting traceability and CI/CD integration.
    - ✅ **Rollback capabilities** — Each change set can be rolled back, which adds resilience in case of deployment errors.
    - ✅ **Standardized across teams** — Liquibase supports YAML, JSON, XML, and SQL formats, offering flexibility for developer preferences.
    - ⚠️ **Learning curve** — Slightly more setup overhead and verbosity compared to simpler tools like Flyway, but benefits outweighed the cost for our use case.
- **PostgreSQL**
    - ✅ **Robust feature set** — Strong ACID compliance, support for JSON, full-text search, and extensions like PostGIS.
    - ✅ **Compatibility with JPA/Hibernate** — Works seamlessly with Spring Data JPA.
    - ✅ **Open-source & widely supported** — Ensures long-term maintainability.
    - ⚠️ **Slightly heavier footprint** than lighter databases like H2, but essential for production realism and data consistency.

---

### 2. **Why Docker**

**Decision:**  
We containerized our application using **Docker** for both development and production environments.

**Rationale & Trade-offs:**

- ✅ **Environment consistency** — Docker ensures the application runs identically across different machines and stages (dev/test/prod).
- ✅ **Simplified onboarding** — New developers can start coding quickly with just Docker and Docker Compose installed.
- ✅ **Infrastructure-as-code** — Our Dockerfiles and Compose files document system dependencies clearly and are version-controlled.
- ✅ **Scalable deployment** — Makes our application easier to deploy to cloud providers or CI/CD pipelines.
- ⚠️ **Added complexity** — Requires learning Docker syntax and understanding container lifecycle.
- ⚠️ **Slight overhead in local dev** — Containers are more resource-intensive than bare-metal setups but acceptable in exchange for consistency.

**Development vs Production Configuration:**

|Environment|Configuration|
|---|---|
|Development|Docker Compose with bind mounts for hot reload, local PostgreSQL container|
|Production|Optimized Docker image, environment variables managed via `.env` or secrets manager, persistent volumes for data|

---

### 3. **Why `Instant` Instead of `LocalDateTime`**

**Decision:**  
We use `java.time.Instant` for timestamps instead of `LocalDateTime`.

**Rationale & Trade-offs:**

- ✅ **Timezone neutrality** — `Instant` represents a point on the timeline in UTC, removing ambiguity caused by time zones.
- ✅ **Better for distributed systems** — Avoids problems during serialization and deserialization in services spread across different time zones.
- ✅ **Aligns with PostgreSQL `TIMESTAMP WITH TIME ZONE`** semantics.
- ⚠️ **Less human-readable** — Requires formatting (e.g., via `DateTimeFormatter`) for display in local time, but this is typically a front-end responsibility.
- ⚠️ **Can require conversion** — Developers must convert to/from `ZonedDateTime` or `LocalDateTime` when local display or input is needed.

---

## Production & Performance
- brief mention on CI/CD pipeline
	- PR proposed to `main` GitHub Action run lint check & build & test for CI
		- team can check any issue from lint & build & test
	- PR merged into `main` branch
	- pre-configured Webhook is triggered to source our codebase to AWS CodePipeline
	- AWS CodePipeline process through AWS CodeBuild and AWS CodeDeploy
	- CodeDeploy to our 2 instances in-place
- logging, monitoring, https, custom domain
	- currently, logging is managed by checking `docker logs` command
	- monitoring is on AWS CloudWatch Dashboard
- performance challenges
	- physical constraints
		- physical distance from south korea to germany
		- CDN
		- Cache
		- Query tuning
	- improvement
		- before index, after index

