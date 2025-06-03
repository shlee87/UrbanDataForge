# Technical Specification Document

## 1. System Architecture

### 1.1 High-Level Architecture
Urban Data Forge follows a modular, service-oriented architecture:
- Data ingestion jobs load public datasets (e.g., CSVs, API responses) into PostgreSQL
- A backend service exposes REST APIs to trigger processing and serve analytic results
- A frontend dashboard fetches data from the backend and visualizes key trends
- Docker Compose manages local services and dependencies

### 1.2 Component Overview
- **Frontend:** React + Chart.js dashboard for data visualization
- **Backend:** REST API built with Java (Spring Boot) for dataset ingestion, processing, and serving analytics
- **Database:** PostgreSQL for raw and transformed dataset storage
- **External Services:** NOAA Weather API, GitHub for code hosting, Docker Hub (optional)

## 2. Technology Stack

### 2.1 Frontend
- **Framework:** React
- **UI Libraries:** Chart.js, Bootstrap (optional)
- **State Management:** React state/hooks
- **Build Tools:** Vite or Create React App

### 2.2 Backend
- **Language:** Java (Spring Boot)
- **Framework:** Spring Boot (Java)
- **API Design:** REST (OpenAPI v3-compatible)
- **Authentication:** Optional token-based auth (JWT or API key header)

### 2.3 Database
- **Type:** PostgreSQL
- **Schema:** Tables for raw data (trips, weather), processed joins, audit logs
- **Backup Strategy:** Manual pg_dump for development; cloud snapshots in production

### 2.4 Infrastructure
- **Hosting:** Local via Docker; deployable to Heroku, Fly.io, or AWS EC2
- **CDN:** None for MVP
- **Monitoring:** Basic logs, Prometheus/Grafana (future stretch)
- **Logging:** stdout + structured logs in backend

## 3. Database Schema

### 3.1 Entity Relationship Diagram
- `trip_data` ← joins → `weather_data` → feeds → `daily_stats`
- `pipeline_logs` tracks job runs and errors

### 3.2 Tables/Collections

#### Table: trip_data
- Fields:
  - `trip_id`: UUID PRIMARY KEY
  - `start_time`: TIMESTAMP NOT NULL
  - `duration_min`: INTEGER
  - `start_station`: TEXT
  - `user_type`: TEXT

#### Table: weather_data
- Fields:
  - `date`: DATE PRIMARY KEY
  - `temperature`: FLOAT
  - `precipitation`: FLOAT
  - `weather_condition`: TEXT

#### Table: daily_stats
- Fields:
  - `date`: DATE PRIMARY KEY
  - `total_rides`: INTEGER
  - `avg_temp`: FLOAT
  - `rides_per_degree`: FLOAT

## 4. API Specifications

### 4.1 Authentication
- **Method:** Bearer token in `Authorization` header
- **Token Type:** JWT (optional for MVP)
- **Security Measures:** Input validation, rate limiting (future), no file system access

### 4.2 Endpoints

#### Endpoint: Upload Trip CSV
- Method: POST
- Path: `/api/v1/upload/trips`
- Request Body: multipart/form-data
- Response:
  ```json
  {
    "status": "success",
    "message": "Trip data uploaded."
  }
  ```

#### Endpoint: Fetch Daily Stats
- Method: GET
- Path: `/api/v1/stats/daily`
- Response:
  ```json
  {
    "status": "success",
    "data": [
      { "date": "2023-08-01", "total_rides": 1300, "avg_temp": 75.2 }
    ]
  }
  ```

## 5. Security Requirements

### 5.1 Authentication & Authorization
- Basic token verification for API access
- (Optional) Role-based access control in future

### 5.2 Data Protection
- Sanitize and validate file uploads
- Avoid exposing internal paths or DB errors

### 5.3 API Security
- Reject oversized payloads
- Enforce schema validation

## 6. Performance Requirements

### 6.1 Response Times
- API Response: ≤ 500ms for aggregated data
- Page Load: ≤ 2 seconds for dashboard

### 6.2 Scalability
- Concurrent Users: 10–50 (MVP); horizontally scalable
- Data Volume: 100K–1M rows per table

## 7. Error Handling

### 7.1 Error Codes
- 400: Bad Request
- 404: Not Found
- 500: Internal Server Error

### 7.2 Logging
- Capture every pipeline step with timestamp
- Log ingestion/upload errors and API usage

## 8. Integration Points

### 8.1 External Services
- **NOAA Weather API**: Daily historical weather fetch
- **Divvy Open Data Portal**: CSV trip downloads

### 8.2 Third-Party APIs
- **Postgres**: JDBC
- **OpenAPI**: Swagger UI for API docs

## 9. Deployment

### 9.1 Environment Setup
- **Development:** Local with Docker
- **Staging:** Optional container on Fly.io or Railway
- **Production:** Optional self-hosted VPS or cloud instance

### 9.2 Deployment Process
- Step 1: Pull latest code from GitHub
- Step 2: Run `docker-compose up` to start services
- Step 3: Access via `localhost:8080` (backend) and `localhost:3000` (frontend)

## 10. Monitoring and Maintenance

### 10.1 Monitoring
- Dataset ingestion success rate
- API uptime and error frequency

### 10.2 Maintenance
- Rotate API tokens if used
- Refresh weather/trip data sources monthly
