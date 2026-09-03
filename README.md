# IvhuRedu Technical Documentation

This repository contains the complete technical documentation for the IvhuRedu platform, an integrated digital agriculture extension system designed to improve communication, coordination, and agricultural service delivery between farmers, Agritex extension workers, and supervisors in Zimbabwe.

The documentation serves as a central knowledge base for developers, DevOps engineers, QA testers, and project stakeholders involved in the IvhuRedu ecosystem.

The documentation covers the platform from architecture and backend development to frontend and mobile applications, authentication, communication services, offline functionality, deployment, testing, and developer standards.

---

## Project Scope

The IvhuRedu technical documentation encompasses the following major areas:

### 1. Platform Architecture
We document the end-to-end IvhuRedu architecture, including the farmer-to-extension-worker workflow, supervisor request management, worker assignment, field reporting, communication services, and offline-first mobile functionality.

The architecture section explains how the mobile application, supervisor web dashboard, FastAPI backend, database, SMS services, and USSD services work together to support agricultural extension services.

### 2. Backend API
We document the FastAPI backend responsible for authentication, user management, farmer requests, extension worker assignments, field reports, SMS services, and other platform operations.

The backend documentation covers API endpoints, request and response schemas, validation, error handling, database interaction, authentication requirements, and service-layer logic.

### 3. Frontend Web Application
We document the Next.js-based supervisor dashboard used by Agritex officers to monitor and manage agricultural extension activities.

The documentation covers the dashboard structure, navigation, authentication flow, farmer request management, worker assignment, field reports, SMS alerts, analytics, settings, API integration, and frontend component organization.

### 4. Frontend Mobile Application
We document the mobile application used by farmers and Agritex extension workers.

The documentation covers the mobile screen structure, navigation flow, authentication, agricultural requests, assigned tasks, task details, field reports, offline data handling, synchronization, and communication workflows.

### 5. Security Architecture
We document the security practices used to protect IvhuRedu users and system data.

This includes authentication, authorization, secure credential management, API protection, input validation, environment variables, role-based access, secure communication, and protection of sensitive user information. Credentials and API keys are not hard-coded into the application and are managed through environment configuration.

### 6. Communication Services
IvhuRedu provides communication mechanisms that support farmers, extension workers, and supervisors, particularly in areas with limited connectivity.

The documentation covers:
- SMS communication
- SMS Leopard integration
- SMS delivery callbacks
- SMS history and monitoring
- SMS broadcasts
- USSD integration
- Extension worker notifications
- Farmer communication workflows

### 7. Offline-First Functionality
IvhuRedu is designed to support agricultural activities in rural areas where internet connectivity may be unreliable.

The documentation covers offline data handling, local storage, synchronization, and the workflow for collecting and submitting information when connectivity becomes available.

### 8. Deployment
We document the deployment requirements and procedures for the IvhuRedu backend, web dashboard, and mobile application.

This section covers environment configuration, production deployment, database configuration, API configuration, and deployment workflows.

### 9. Developer Guide
We document development standards and contribution practices used by the IvhuRedu team.

This includes:
- Project structure
- Naming conventions
- Coding standards
- API development practices
- Testing strategies
- Git workflow
- Commit conventions
- Pull requests
- Environment configuration
- Secure development practices

---

## Documentation Structure

The documentation is organized into the following text sections:

*   **Overview:** Product vision, problem statement, target users, and key features.
*   **Architecture:** System components, workflows, data flow, and technology decisions.
*   **Backend:** FastAPI architecture, APIs, schemas, services, repositories, and database integration.
*   **Frontend Web:** Next.js supervisor dashboard structure and API integration.
*   **Frontend Mobile:** Mobile application structure, navigation, offline functionality, and synchronization.
*   **Security:** Authentication, authorization, validation, and data protection.
*   **Communication:** SMS, USSD, SMS Leopard integration, notifications, and delivery tracking.
*   **Offline Support:** Offline data collection, local storage, and synchronization.
*   **Deployment:** Environment configuration and production deployment.
*   **Testing:** Backend, frontend, mobile, API, and integration testing.
*   **Developer Guide:** Coding standards, Git workflow, contribution practices, and development guidelines.

---

## Technology Stack

The documentation covers the following operational layers and technical choices:

*   **Backend Framework:** FastAPI with Python
*   **Database:** PostgreSQL
*   **Object Relational Mapping:** SQLAlchemy
*   **API Validation:** Pydantic
*   **Web Frontend:** React with Next.js
*   **Mobile Frontend:** Flutter
*   **Local Mobile Storage:** SQLite
*   **SMS Gateway:** SMS Leopard
*   **USSD Gateway:** Africa's Talking
*   **Location Services:** LocationIQ
*   **Authentication:** Token-based authentication and OTP
*   **API Communication:** REST APIs
*   **Web Application:** Next.js
*   **Mobile Platform:** Android
*   **Version Control:** Git and GitHub

---

## Key Accomplishments

Throughout the IvhuRedu development and documentation process, the team achieved the following:

- Documented the overall IvhuRedu system architecture and workflows
- Documented the farmer, extension worker, and supervisor roles
- Documented backend API endpoints and data structures
- Established a FastAPI backend architecture
- Implemented database models using SQLAlchemy
- Implemented validation using Pydantic schemas
- Separated business logic into service layers
- Implemented farmer agricultural request workflows
- Implemented extension worker assignment workflows
- Supported assignment of nearby extension workers based on location
- Developed the supervisor dashboard for managing farmer requests
- Developed mobile interfaces for farmers and extension workers
- Implemented field reporting workflows
- Integrated SMS communication through SMS Leopard
- Implemented SMS broadcast functionality
- Implemented SMS history and summary monitoring
- Implemented SMS delivery callback handling
- Integrated USSD communication capabilities
- Designed the system to support unreliable rural connectivity
- Documented secure environment and credential management
- Established testing and developer documentation standards

---

## Live Documentation

The IvhuRedu technical documentation will be made available through the project's documentation deployment environment.

- **Live:** To be added after documentation deployment.

---

## Repository Contents

The documentation repository contains the following:

- Complete Markdown documentation for the IvhuRedu platform
- Platform overview and product documentation
- System architecture documentation
- Backend API documentation
- Frontend web documentation
- Frontend mobile documentation
- Security documentation
- Communication and SMS documentation
- Offline functionality documentation
- Deployment documentation
- Testing documentation
- Developer guide
- Documentation configuration
- Custom styling and assets
- CI/CD configuration where applicable
- README with project overview and documentation links

---

## Contributors

This documentation was prepared by the **NYEREDZI** team as part of the IvhuRedu platform development initiative. IvhuRedu was developed as a digital agricultural extension solution.
