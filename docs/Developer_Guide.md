# 8. Developer Guide

This guide provides development standards and workflows for developers contributing to the IvhuRedu project.

It covers:

- Global Code Standards
- Testing Conventions
- Git Workflow
- Glossary

The purpose of this guide is to keep development consistent, organized, and easy for team members to maintain.

---

## 8.1 Global Code Standards

IvhuRedu follows consistent coding practices across the backend, web dashboard, and mobile application.

### General Standards

Developers working on IvhuRedu should:

- Write clear and readable code.
- Use meaningful names for variables, functions, classes, and components.
- Keep functions and components focused on one responsibility.
- Avoid unnecessary duplicate code.
- Follow the existing project structure.
- Keep related functionality organized.
- Handle errors appropriately.
- Test changes before creating a Pull Request.
- Never commit passwords, API keys, tokens, or other sensitive information.
- Keep commits focused on related changes.

### Backend

The backend uses **Python and FastAPI**.

The backend separates responsibilities into areas such as:

- **Routers** — handle API endpoints and HTTP requests.
- **Schemas** — validate request and response data using Pydantic.
- **Services** — contain application and business logic.
- **Repositories** — handle database operations where applicable.
- **Models** — represent database data using SQLAlchemy.

### Web

The web dashboard uses **React and Next.js**.

Developers should:

- Create reusable components where appropriate.
- Keep components focused on specific responsibilities.
- Use meaningful names.
- Keep API integration organized.
- Handle loading and error states.
- Follow the existing styling and UI structure.
- Avoid unnecessary duplicate code.

### Mobile

The mobile application uses **Flutter and Dart**.

Developers should:

- Use meaningful names for widgets, variables, and functions.
- Keep widgets focused on their responsibilities.
- Reuse widgets where appropriate.
- Separate UI code from business logic.
- Handle loading and error states.
- Consider offline functionality.
- Keep local storage and synchronization logic organized.

---

## 8.2 Testing Conventions

Testing should be performed before changes are submitted for review.

Testing should focus on the part of the system that has been changed.

### Backend Testing

Backend changes should verify that:

- API endpoints work correctly.
- Valid requests return expected responses.
- Invalid requests are handled correctly.
- Authentication works correctly.
- Database operations work correctly.
- Expected errors are handled properly.

### API Testing

API endpoints should be tested with valid and invalid inputs, including:

- Request parameters
- Request body
- Authentication
- HTTP status codes
- Response data
- Error responses

### Web Testing

Web dashboard changes should be checked to confirm that:

- Pages load correctly.
- Components display correctly.
- API data is displayed correctly.
- Forms work correctly.
- Loading states work correctly.
- Error states work correctly.
- Navigation works correctly.

### Mobile Testing

Mobile changes should be checked for:

- Authentication
- Forms
- User interactions
- API communication
- Offline functionality where applicable
- Local data storage
- Data synchronization
- Error handling

### Integration Testing

When a change affects multiple components, the complete workflow should be tested.

For example:

```text
Flutter Mobile Application
          ↓
FastAPI Backend
          ↓
PostgreSQL Database
          ↓
FastAPI Backend
          ↓
Flutter Mobile Application
```

For the web dashboard:

```text
Next.js Web Dashboard
          ↓
FastAPI Backend
          ↓
PostgreSQL Database
          ↓
Next.js Web Dashboard
```

### Before a Pull Request

Before opening a Pull Request, developers should:

- Run relevant tests.
- Check for errors.
- Review changed files.
- Check formatting.
- Remove unnecessary code.
- Make sure no secrets have been committed.
- Confirm that the feature works as expected.

---

## 8.3 Git Workflow

IvhuRedu uses **Git and GitHub** for version control and collaboration.

Developers should use feature branches rather than making changes directly to the `main` branch.

### 1. Update the Main Branch

Before starting new work:

```bash
git checkout main
git pull origin main
```

### 2. Create a Feature Branch

Create a branch for the task you are working on:

```bash
git checkout -b feature/your-feature-name
```

Example:

```bash
git checkout -b feature/overview-architecture
```

Examples of branch names:

```text
feature/overview
feature/architecture
feature/backend-api
feature/settings
feature/mobile-auth
fix/login-error
docs/developer-guide
```

### 3. Make Changes

After making changes, check the repository status:

```bash
git status
```

### 4. Review Changes

Review the changes before committing:

```bash
git diff
```

Make sure that:

- The changes are related to your task.
- No unnecessary files were changed.
- No sensitive information was added.
- The code or documentation is correct.

### 5. Stage Changes

Add the files you want to commit:

```bash
git add <file-name>
```

For multiple files:

```bash
git add docs/Overview.md docs/Architecture.md
```

### 6. Commit Changes

Use a clear commit message:

```bash
git commit -m "docs: add developer guide"
```

Examples:

```text
docs: add overview
docs: add architecture documentation
docs: add developer guide
feat: add farmer request workflow
fix: resolve authentication error
test: add API tests
```

### 7. Push the Branch

Push the branch to GitHub:

```bash
git push -u origin feature/your-feature-name
```

### 8. Create a Pull Request

After pushing the branch:

1. Open the GitHub repository.
2. Select the branch you pushed.
3. Create a Pull Request.
4. Add a clear title.
5. Describe the changes.
6. Mention the testing performed.
7. Request review from the appropriate team members.

### Pull Request Example

**Title:**

```text
docs: add developer guide
```

**Description:**

```text
## Changes

- Added global code standards.
- Added testing conventions.
- Added Git workflow.
- Added project glossary.

## Testing

- Reviewed the documentation.
- Checked Markdown formatting.
- Confirmed the content follows the IvhuRedu development workflow.
```

### 9. Keep the Branch Updated

If the `main` branch changes while you are working:

```bash
git checkout main
git pull origin main
```

Return to your feature branch:

```bash
git checkout feature/your-feature-name
```

Update your branch according to the team's preferred workflow:

```bash
git merge main
```

Resolve any conflicts if they occur, then test the changes again before pushing.

---

## 8.4 Glossary

| Term | Definition |
|---|---|
| **API** | Application Programming Interface. A way for different software components to communicate. |
| **Agritex** | Zimbabwe's agricultural extension service that provides agricultural support to farmers. |
| **Backend** | The server-side part of the system responsible for application logic, APIs, and data processing. |
| **Database** | A system used to store and manage application data. |
| **FastAPI** | A Python web framework used to build the IvhuRedu backend API. |
| **Flutter** | A framework used to build the IvhuRedu mobile application. |
| **Frontend** | The part of an application that users interact with directly. |
| **Git** | A version control system used to track changes to source code and documentation. |
| **GitHub** | A platform used to host Git repositories and support collaboration. |
| **HTTP** | Hypertext Transfer Protocol, used for communication between clients and servers. |
| **IvhuRedu** | The digital agriculture extension system developed to improve agricultural service delivery. |
| **Next.js** | A React framework used to build the IvhuRedu web dashboard. |
| **Offline-First** | An approach where an application can continue supporting important functionality when internet connectivity is unavailable. |
| **ORM** | Object-Relational Mapping, a technique for interacting with relational databases using programming objects. |
| **PostgreSQL** | The relational database management system used by IvhuRedu. |
| **Pydantic** | A Python library used for data validation and schema definition in the backend. |
| **Pull Request** | A GitHub process used to propose changes and request review before merging them. |
| **REST API** | An API style that uses HTTP methods and resources for communication between software systems. |
| **SQLAlchemy** | A Python SQL toolkit and ORM used for database interaction. |
| **SMS** | Short Message Service, used to send text messages to mobile phones. |
| **USSD** | Unstructured Supplementary Service Data, a mobile communication method that does not require internet access. |
| **Supervisor** | An IvhuRedu user responsible for managing requests, assigning extension workers, and monitoring activities. |
| **Extension Worker** | An Agritex worker who receives requests, performs field activities, and submits field reports. |
| **Synchronization** | The process of transferring locally stored mobile data to the backend when connectivity becomes available. |
| **Environment Variables** | Configuration values stored outside source code, commonly used for credentials and application settings. |
| **Authentication** | The process of verifying the identity of a user. |
| **Role-Based Access** | A security approach where access to functionality depends on a user's assigned role. |
| **Agricultural Request** | A request submitted by a farmer for agricultural support or extension services. |
| **Field Report** | Information submitted by an extension worker after carrying out an agricultural field activity. |