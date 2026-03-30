# LogShield - Intelligent Log Analytics and Threat Detection Engine

## Overview

BTech CSE 4th Semester PBL Project.
A Java Spring Boot application that generates, collects, parses, and analyzes application logs in real-time, with a rule-based cybersecurity threat detection engine.

## Tech Stack

- **Language**: Java 17 (GraalVM 22.3 / Java 19 runtime)
- **Framework**: Spring Boot 3.2.3
- **Web**: Spring MVC + Thymeleaf (server-side HTML rendering)
- **Database**: H2 In-Memory DB via Spring Data JPA / Hibernate
- **Security**: Spring Security (disabled for dev, custom auth via session)
- **Build**: Apache Maven 3.8.6
- **Frontend**: Plain HTML5 + CSS3 + Vanilla JavaScript (no React, no TypeScript)

## Project Structure

```
log-analytics/
├── pom.xml                                         # Maven dependencies
├── src/main/java/com/loganalytics/
│   ├── LogAnalyticsApplication.java                # Spring Boot entry point
│   ├── model/
│   │   ├── LogEntry.java                           # Log record entity
│   │   ├── ThreatAlert.java                        # Threat alert entity
│   │   └── AppUser.java                            # User entity
│   ├── repository/
│   │   ├── LogEntryRepository.java                 # JPA repo for logs
│   │   ├── ThreatAlertRepository.java              # JPA repo for alerts
│   │   └── AppUserRepository.java                  # JPA repo for users
│   ├── service/
│   │   ├── LogService.java                         # Log CRUD + analytics
│   │   ├── ThreatAlertService.java                 # Alert management
│   │   └── UserService.java                        # User auth logic
│   ├── engine/
│   │   └── ThreatDetectionEngine.java              # Core detection rules
│   ├── controller/
│   │   ├── AuthController.java                     # Login/Register/Logout API
│   │   ├── ActivityController.java                 # User activity + attack sim
│   │   ├── DashboardController.java                # Admin dashboard API
│   │   └── PageController.java                     # HTML page routing
│   ├── dto/
│   │   ├── LoginRequest.java
│   │   ├── RegisterRequest.java
│   │   └── ApiResponse.java
│   └── config/
│       ├── SecurityConfig.java                     # Spring Security config
│       └── DataInitializer.java                    # Seed demo users
├── src/main/resources/
│   ├── application.properties                      # App configuration
│   ├── templates/                                  # Thymeleaf HTML pages
│   │   ├── index.html                              # Landing page
│   │   ├── login.html                              # Login page
│   │   ├── register.html                           # Registration page
│   │   ├── user-dashboard.html                     # User activity panel
│   │   └── admin-dashboard.html                    # Admin log viewer
│   └── static/
│       ├── css/main.css                            # Global styles
│       ├── css/admin.css                           # Admin dashboard styles
│       ├── js/user-dashboard.js                    # User panel logic
│       └── js/admin-dashboard.js                   # Admin panel logic
```

## Threat Detection Rules (Engine)

1. **Brute Force** — 5+ failed logins from same IP in 5 minutes → HIGH alert
2. **SQL Injection** — Regex pattern matches on request params/body → CRITICAL alert
3. **XSS Attack** — Script/event handler patterns in form data → HIGH alert
4. **Path Traversal** — `../` patterns in URL or params → HIGH alert
5. **DDoS/Rate Limit** — 50+ requests/min from same IP → CRITICAL alert

## Running

The app runs via Maven:
```bash
cd log-analytics && PORT=8099 mvn spring-boot:run
```

Served on port **8099**. The "Start application" workflow handles this.

## Demo Users

| Username | Password    | Role  |
|----------|-------------|-------|
| admin    | admin123    | ADMIN |
| alice    | password123 | USER  |
| bob      | pass456     | USER  |
| charlie  | charlie789  | USER  |

## Pages

- `/` — Landing page with live log feed and system stats
- `/login` — Login (generates AUTH logs on every attempt)
- `/register` — Registration (generates AUTH logs)
- `/dashboard` — User activity panel (search, forms, attack simulator)
- `/admin` — Admin dashboard (live logs, threat alerts, charts, IP analysis)

## API Endpoints

- `POST /api/auth/login` — Authenticate user
- `POST /api/auth/register` — Register new user
- `POST /api/auth/logout` — Logout
- `GET /api/auth/session` — Check session
- `POST /api/activity/search` — Log a search event
- `POST /api/activity/submit-form` — Log form submission
- `GET /api/activity/page-visit` — Log page visit
- `POST /api/activity/download` — Log file download
- `POST /api/activity/simulate-attack` — Trigger attack simulation
- `GET /api/dashboard/stats` — Log statistics
- `GET /api/dashboard/threats` — Threat summary
- `GET /api/dashboard/alerts` — All/filtered alerts
- `POST /api/dashboard/alerts/{id}/resolve` — Resolve an alert
- `GET /api/dashboard/logs` — Recent log entries
