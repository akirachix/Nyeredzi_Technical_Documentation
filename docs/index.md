# IvhuRedu Technical Documentation

Welcome to the IvhuRedu Technical Documentation. This site contains everything developers need to understand, build, test, and deploy the IvhuRedu agricultural platform.

## What is IvhuRedu

IvhuRedu is a digital agricultural platform that connects farmers and Agritex officers. The name means "Our Soil." It improves how agricultural information is collected, shared, managed, and monitored in the field.

The platform provides three ways to access its services:

- **Farmers** use USSD on basic mobile phones without internet
- **Extension Workers** use a Flutter mobile app on smartphones for field work
- **Supervisors and Administrators** use a Next.js web dashboard on computers

All three channels connect through a single backend API and a centralized PostgreSQL database.

## Documentation Structure

This documentation is organized into eight main sections:

| Section | What It Covers |
|---|---|
| [1. Overview](Overview.md) | Product definition, problem statement, target users, key features, and workflow |
| [2. Architecture](Architecture.md) | Core principles, system diagram, components, data flow, design, and scalability |
| [3. Backend / API](Backend_API.md) | API overview, setup, architecture layers, endpoints, data models, testing, and deployment |
| [4. Frontend Web](Frontend_Web.md) | Dashboard technology, setup, structure, authentication, features, styling, and deployment |
| [5. Frontend Mobile](Frontend_Mobile.md) | Mobile app technology, structure, components, security, installation, and troubleshooting |
| [6. Security](Security.md) | Security posture, data classification, network, application, data, webhook, and incident response |
| [7. Deployment](Deployment.md) | Deployment architecture, backend, frontend, mobile, external services, environment variables, and CI/CD |
| [8. Developer Guide](Developer_Guide.md) | Code standards, testing conventions, Git workflow, and glossary |

## Quick Links

- [Backend API Setup](Backend_API.md#setup-and-installation)
- [Frontend Web Setup](Frontend_Web.md#setup-and-installation)
- [Mobile App Setup](Frontend_Mobile.md#installation)
- [Environment Variables](Deployment.md#environment-variables)
- [Git Workflow](Developer_Guide.md#git-workflow)
- [Glossary](Developer_Guide.md#glossary)

## Getting Started

If you are new to the project, start with the [Overview](Overview.md) to understand what IvhuRedu does and who it serves. Then read the [Architecture](Architecture.md) to understand how the system is built. Finally, follow the setup instructions for the part of the platform you will be working on.