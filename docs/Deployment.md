# 7. Deployment

This page describes how IvhuRedu is deployed from development to production. It covers the overall deployment architecture, how the backend is deployed, how the frontend is deployed, how the mobile app is deployed, the external services used, how system integration works, the environment variables required, and the continuous integration and delivery pipeline.

---

## Deployment Architecture

The deployment architecture defines how code moves from a developer's computer to the live production environment.

All backend deployments flow from the `main` branch on GitHub into Heroku. When a developer completes a feature, they push their branch to GitHub and open a pull request. After review and approval, the code is merged into the `main` branch. Heroku detects the change to `main`, pulls the latest code, and deploys it automatically.

The frontend web dashboard follows a similar pattern. Code is merged into `main` and then deployed to the hosting platform. The mobile application is built from the `main` branch and distributed to devices.

The database runs as a managed service connected to the backend. External services such as Africa's Talking, SMS Leopard, and LocationIQ are configured independently and connected to the backend through API keys.

---

## Backend Deployment

The backend is deployed to Heroku.

### Platform

Heroku provides both staging and production environments. The staging environment is used for testing changes before they reach production. The production environment serves live users.

### Server

The backend runs as an ASGI application. Uvicorn is the ASGI server used to run the FastAPI application. Alternatively, Gunicorn with Uvicorn workers can be used.

### Deploy Branch

Deployments are triggered from the `main` branch. Only code that has been reviewed and merged into `main` is deployed.

### Procfile

The Procfile tells Heroku how to start the application. It contains the command to run the ASGI server. The command specifies the host as `0.0.0.0` to accept connections from any network interface and uses the `PORT` environment variable provided by Heroku.

### Scaling

Heroku dynos scale automatically based on live traffic. If the number of requests increases, Heroku can add more dyno instances to handle the load. If traffic decreases, dynos can scale down to save resources.

### Rollback

If a deployment causes problems, you can roll back to a previous stable release directly from the Heroku dashboard. This allows quick recovery without needing to revert code changes in GitHub.

### Deployment Steps

Development happens on feature branches. Developers create a branch from `main`, make their changes, and test locally. They push the branch to GitHub and open a pull request. Team members review the code. Once at least three reviewers approve and all checks pass, the code is merged into `main`. Heroku detects the merge, pulls the latest code, and starts the ASGI server using the Procfile. Environment variables and secrets are injected at runtime from the Heroku dashboard. Heroku dynos scale automatically based on traffic. If something goes wrong, the team can roll back to a previous stable release from the Heroku dashboard.

---

## Frontend Deployment

The frontend consists of two parts: the informational website and the web dashboard.

### Informational Website

The informational website is hosted on Vercel. The live URL is provided in the project configuration. The site provides public information about IvhuRedu, including the product description, features, team information, and contact details.

### Web Dashboard

The web dashboard is built with Next.js. The exact deployment target and pipeline for the dashboard should be documented once confirmed. It may be deployed to Vercel, Netlify, or another platform that supports Next.js applications.

### Deployment Process

The frontend deployment follows the same branch-based workflow as the backend. Code is reviewed through pull requests, merged into `main`, and then deployed to the hosting platform. Environment variables for the frontend such as the backend API URL are configured in the hosting platform dashboard.

---

## Mobile Deployment

The mobile application is built with Flutter. It is compiled to native Android code.

### Build Process

The Flutter project is built using the Flutter build command. This compiles the Dart code into an Android application package. The build process uses environment-specific configuration to connect to the correct backend API.

### Distribution

The application can be distributed through Google Play for public availability. It can also be distributed internally through enterprise app distribution channels. The exact distribution details including build signing and the release process should be documented once confirmed.

### Versioning

Each release of the mobile app has a version number. The version number is incremented for each release. Users receive updates through the distribution channel they used to install the app.

---

## External Services

The platform relies on three external services.

### Africa's Talking

Africa's Talking provides USSD and SMS communication for farmers. The backend sends USSD responses and SMS messages through the Africa's Talking API. The backend receives USSD session callbacks from Africa's Talking at a registered webhook URL.

### SMS Leopard

SMS Leopard sends SMS alerts and notifications. The backend calls the SMS Leopard API to send messages. SMS Leopard may send delivery status callbacks to the backend to confirm whether messages were delivered.

### LocationIQ

LocationIQ provides geocoding and location-based lookups. The backend calls the LocationIQ API to convert addresses to coordinates, convert coordinates to addresses, and provide address autocomplete suggestions.

---

## System Integration

The backend is the only component that talks directly to external services. The web dashboard and mobile app never call Africa's Talking, SMS Leopard, or LocationIQ directly. Every request from the user-facing applications passes through the backend API first. The backend then calls the external service if needed and returns the result to the user-facing application.

This design keeps external service credentials secure. Only the backend stores API keys for Africa's Talking, SMS Leopard, and LocationIQ. The mobile app and web dashboard do not need these keys. It also centralizes error handling. If an external service fails, the backend can handle the error and return a meaningful message to the user.

---

## Environment Variables

Environment variables are configuration values that the system reads at runtime. They control how the platform connects to databases and external services.

### Africa's Talking

| Variable | Purpose | Where to Get It |
|---|---|---|
| `AT_API_KEY` | Authenticates API calls to Africa's Talking | Africa's Talking developer portal. Use the sandbox key for local development. |
| `AT_USERNAME` | Identifies the Africa's Talking account | Africa's Talking developer portal |

The webhook URL registered with Africa's Talking must be a publicly reachable HTTPS endpoint. For local development, a tunneling service such as ngrok can provide a temporary public URL. Confirm that only the USSD product is enabled if SMS now runs through SMS Leopard.

### SMS Leopard

| Variable | Purpose | Where to Get It |
|---|---|---|
| `SMSLEOPARD_API_KEY` | Authenticates SMS send requests | SMS Leopard dashboard |
| `SMSLEOPARD_API_SECRET` | Paired with the API key for authentication | SMS Leopard dashboard |

Confirm the exact credential field names that the SMS Leopard API expects. Confirm the sender ID configured for IvhuRedu. Confirm whether a delivery status webhook needs to be registered and what its URL should be.

### LocationIQ

| Variable | Purpose | Where to Get It |
|---|---|---|
| `LOCATIONIQ_API_KEY` | Authenticates geocoding requests | LocationIQ dashboard |

Confirm the plan or tier in use and its rate limit. This prevents local development from unexpectedly hitting the rate limit during testing.

### Database

Define the `DATABASE_URL` format and any PostgreSQL-specific setup. This includes required extensions such as PostGIS for spatial data and connection pooling configuration.

### Adding a New Integration

When adding a new external service, follow the team convention. Add the credential to `.env.example` with a placeholder value. Document it on this page in the same table format. Add a troubleshooting entry once a real failure has been encountered.

---

## CI/CD Pipeline

Continuous Integration and Continuous Delivery automate the process of testing and deploying code.

### Version Control

GitHub is used for version control and collaborative development. All code lives in the GitHub repository. Developers create branches, push changes, and open pull requests.

### Pull Requests

Code changes go through pull requests. Every pull request must be reviewed before merging. At least three reviewers must approve a pull request. Pull request descriptions should summarize the changes and link related issues.

### Automated Checks

Before a pull request is merged, tests, linting, and formatting checks should pass.

| Area | Check | Command |
|---|---|---|
| Backend | Tests | `pytest` |
| Frontend | Tests | `npm test` |
| Frontend | Formatting | `npm run format` |
| Mobile | Tests | `flutter test` |
| Mobile | Analysis | `flutter analyze` |

### Deployment Trigger

Merging approved code into the `main` branch triggers deployment. For the backend, Heroku detects the merge and deploys automatically. For the frontend, the hosting platform deploys the updated code. For the mobile app, a new build is generated.

### Documentation

Automated CI/CD pipelines should only be documented once they are actually configured for a given component. Document the exact pipeline steps, the tools used, and the expected behavior.