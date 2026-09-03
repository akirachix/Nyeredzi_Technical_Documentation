# 3. Backend / API

This page describes the IvhuRedu backend and REST API. It covers what the API does, where it is hosted, what you need to start developing, how to set it up, how it is organized, the rules it follows, the available endpoints, the data structures it uses, how to test it, the code standards it follows, and how to deploy it.

---

## API Overview

The IvhuRedu backend is a REST API built with FastAPI. It serves as the central communication layer between all parts of the platform. The USSD service, the mobile application, and the web dashboard all send requests to this API. The API processes those requests, interacts with the PostgreSQL database, calls external services when needed, and returns responses.

The API handles authentication, user management, farmer registration, extension worker management, location services, farmer requests, field reports, image uploads, SMS messaging, and USSD session processing. It enforces role-based access control so that each user can only access the data and functions allowed by their role.

---

## Hosted API

The live API is hosted on Heroku. The base URL for the production API is provided in the project configuration. The API documentation is generated automatically by FastAPI and is available at the `/docs` endpoint. This interactive documentation, powered by Swagger UI, allows developers to explore every endpoint, see the expected request and response formats, and send test requests directly from the browser.

The API also exposes a `/health` endpoint that returns the current status of the server. Monitoring tools can call this endpoint to check whether the API is running and healthy.

---

## Prerequisites

Before you can set up the backend for development, you need the following tools and accounts installed on your computer.

| Requirement | Purpose |
|---|---|
| Python 3.9 or higher | The programming language for the backend |
| pip | For installing Python packages |
| Git | For version control |
| PostgreSQL | Local or remote database for storing data |
| Africa's Talking account | For testing USSD and SMS features |
| LocationIQ API key | For testing geocoding and address lookup |

For local development, you can use a PostgreSQL instance running in Docker or installed directly on your machine. You will also need a tool like ngrok if you want to test USSD webhooks locally, because Africa's Talking requires a publicly reachable HTTPS endpoint.

---

## Setup and Installation

Follow these steps to set up the backend on your local machine.

1. Clone the repository from GitHub and navigate to the backend directory.

2. Create a Python virtual environment to isolate the project dependencies.

3. Activate the virtual environment.

4. Install the required Python packages using the requirements file.

5. Create a `.env` file in the backend directory. Copy the `.env.example` file and fill in the actual values for your database URL, Africa's Talking credentials, LocationIQ API key, and other secrets.

6. Run the database migrations to create the required tables.

7. Start the development server using Uvicorn.

The server will start on your local machine and you can access the API at `http://localhost:8000`. The interactive documentation will be available at `http://localhost:8000/docs`.

---

## Architecture Layers

The backend code is organized into layers that separate different responsibilities.

### Routers

Routers define the API endpoints. Each router handles a group of related endpoints. For example, the auth router handles login and password management. The farmers router handles farmer registration and updates. Routers receive HTTP requests, validate input using Pydantic models, call the appropriate service functions, and return HTTP responses.

### Services

Services contain the business logic. A service function performs a specific task such as creating a farmer, assigning a request, or sending an SMS. Services interact with the database through the models layer. They also call external APIs such as Africa's Talking or LocationIQ when needed.

### Models

Models define the database tables and the data structures used by the API. Database models use SQLAlchemy or SQLModel to map Python classes to PostgreSQL tables. Pydantic schemas define the shape of request and response data. They validate incoming JSON and serialize outgoing data.

### Dependencies

Dependencies are reusable pieces of logic that routers and services can depend on. The most common dependency is authentication. When a protected endpoint is called, the authentication dependency extracts the token from the request header, validates it, and returns the current user. If the token is invalid or missing, the dependency raises an error.

---

## API Conventions

The API follows standard REST conventions and project-specific rules.

### HTTP Methods

| Method | Purpose |
|---|---|
| POST | Create a new resource |
| GET | Read or retrieve a resource |
| PUT | Replace a resource completely |
| PATCH | Update a resource partially |
| DELETE | Remove a resource |

### URL Structure

Endpoint paths use lowercase letters and hyphens. They are organized by resource. For example, all farmer-related endpoints start with `/farmers/`. All location-related endpoints start with `/locations/`.

### Request and Response Format

All requests and responses use JSON. Dates are formatted as ISO strings. UUIDs are used as identifiers instead of sequential numbers for security.

### Status Codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 201 | Resource was created |
| 400 | Request input was invalid |
| 401 | Authentication token is missing or invalid |
| 403 | User does not have permission |
| 404 | Requested resource does not exist |
| 409 | Conflict, such as a duplicate username or email |
| 500 | Unexpected server error |

### Naming

Variables, functions, and file names use `snake_case`. Class names, Pydantic models, and database models use `PascalCase`.

---

## Endpoint Categories

The API provides endpoints organized into the following categories.

### Authentication

These endpoints handle user login, password changes, password recovery, and token refresh. The login endpoint returns an access token, a refresh token, and the user's role.

### Users

These endpoints manage system users such as administrators and supervisors. They support creating, reading, updating, and deleting user accounts.

### Farmers

These endpoints manage farmer records. They support registering new farmers, retrieving farmer details, updating farmer information, and deleting farmer records.

### Extension Workers

These endpoints manage Extension Worker accounts. They support creating workers, updating their status, retrieving worker information, and deleting workers.

### Locations

These endpoints manage geographic data. They support saving locations, searching by text or ward, geocoding addresses, reverse geocoding coordinates, finding nearby workers or farmers, and managing location records.

### Farmer Requests

These endpoints manage assistance requests from farmers. They support creating requests, batch creation, duplicate cleanup, searching by USSD keyword, assigning to workers, and managing request status.

### Field Images

These endpoints manage images attached to field reports. They support uploading images, downloading decrypted images, retrieving images by report, and managing image records.

### Field Reports

These endpoints manage field reports submitted by Extension Workers. They support creating reports, retrieving reports by worker, updating reports, and deleting reports.

### SMS

These endpoints manage SMS communication. They support sending individual messages, sending alerts, sending OTPs, verifying OTPs, retrieving logs and history, and receiving delivery callbacks.

### USSD and System

These endpoints handle USSD callbacks from Africa's Talking and provide system health checks.

---

## Data Models

The API uses two types of data models. Pydantic models define the structure of data sent to and received from the API. SQLAlchemy or SQLModel database models define the structure of tables in the PostgreSQL database.

### Pydantic Schemas

Pydantic schemas are Python classes that define the expected shape of JSON data. They specify which fields are required, which are optional, and what data types are allowed. When a request arrives, FastAPI uses the Pydantic schema to validate the JSON body. If the data does not match the schema, the API returns a 400 error with details about what was wrong.

### Database Models

Database models are Python classes that map to PostgreSQL tables. Each class represents one table. Each attribute represents one column. Relationships between tables are defined using foreign keys and relationship attributes. The models use UUIDs as primary keys. Spatial data uses PostGIS geometry types.

---

## Testing and QA

The backend has a comprehensive testing strategy to ensure reliability.

### Unit Tests

Unit tests check individual functions and methods in isolation. They verify that a single piece of logic produces the correct output for a given input. Unit tests do not depend on the database or external services.

### Integration Tests

Integration tests check that different parts of the backend work together correctly. They test API endpoints using an HTTP client. They verify that requests flow through the router, service, and model layers correctly. They test database operations and external service interactions.

### Test Runner

Tests are run using `pytest`. The HTTP client used for integration tests is `httpx.AsyncClient`. Test coverage is measured using `pytest-cov`.

### Coverage Requirement

Backend test coverage must remain above 90 percent. This means at least 90 percent of the backend code must be executed during tests. If a pull request reduces coverage below this threshold, the developer must add more tests before merging.

---

## Code Standards

The backend follows strict code standards to maintain quality and consistency.

### Naming Conventions

Use `snake_case` for variables, functions, and file names. Use `PascalCase` for classes, Pydantic models, and SQLAlchemy or SQLModel database models.

### File Organization

Keep Pydantic schemas, database models, routers, and dependencies in separate files unless they are tightly coupled and splitting them would reduce clarity.

### Linting

Use standard Python syntax and style checkers to ensure the code follows Python conventions.

### Documentation

Use Python docstrings for all functions, classes, and modules. Update the README and feature documentation whenever you make a change that affects them.

### Security

Validate all inputs using Pydantic. Handle authentication using FastAPI security dependencies with JWT tokens. Use CORSMiddleware and TrustedHostMiddleware to restrict which origins can access the API.

---

## Deployment

The backend is deployed to Heroku.

### Platform

Heroku provides both staging and production environments. The server runs as an ASGI application using Uvicorn or Gunicorn with Uvicorn workers.

### Branch

Deployments are triggered from the `main` branch. When code is merged into `main`, Heroku automatically pulls the latest code and deploys it.

### Procfile

The Procfile tells Heroku how to start the application. It contains the command to run the ASGI server. The command specifies the host as `0.0.0.0` to accept connections from any network interface and uses the `PORT` environment variable provided by Heroku.

### Environment Variables

Sensitive values such as database URLs and API keys are managed securely in the Heroku dashboard. They are injected at runtime and never committed to the code repository. Locally, environment variables are parsed and validated using Pydantic-Settings.

### Scaling

Heroku dynos scale automatically based on traffic. If the number of requests increases, Heroku can spin up additional server instances to handle the load.

### Rollback

If a deployment causes problems, you can roll back to a previous stable release directly from the Heroku dashboard. This allows quick recovery from bad deployments.

### Steps

Development happens on feature branches. Code is reviewed through pull requests. When approved, it is merged into `main`. Heroku detects the merge, pulls the code, and starts the server using the Procfile. Environment variables are injected at runtime. Dynos scale based on demand. Rollback is available if needed.