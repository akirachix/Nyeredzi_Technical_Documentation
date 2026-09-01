# 5. Frontend Mobile

This page describes the IvhuRedu mobile application. It covers what the app is, the technologies used, how the project is organized, the architecture layers, the core components, the security measures, how to install it, and how to troubleshoot common problems.

---

## Overview

The IvhuRedu mobile application is built with Flutter and Dart. It is designed for Agritex Extension Workers who use Android smartphones in the field. The application supports authentication, farmer management, field activities, assignments, and reporting.

The app communicates with the IvhuRedu backend through REST API calls. It exchanges data such as user credentials, farmer records, assignment details, and field reports. The app is designed to work offline when network coverage is poor and synchronize with the backend when connectivity returns.

Target users are Extension Workers. The primary purpose is to support these workers in managing farmers, field activities, assignments, and reports while they are away from the office.

---

## Tech Stack

The mobile application uses the following technologies.

| Piece | Technology | Purpose |
|---|---|---|
| Framework | Flutter | A toolkit for building natively compiled applications for mobile from a single codebase |
| Language | Dart | The programming language used to write Flutter applications |
| Testing | Flutter Test | Provides unit tests and widget tests for application logic and user interface components |
| Linting | Dart Format + Flutter Analyze | Dart Format ensures consistent code formatting. Flutter Analyze checks for code quality issues |
| Documentation | Dartdoc | A standard for writing documentation comments in Dart code |

---

## Project Structure

The mobile application code lives in the `mobile/` directory at the root of the repository. This directory contains the full Flutter project.

The project structure follows Flutter conventions.

| Directory | Contents |
|---|---|
| `lib/` | Main application code |
| `lib/screens/` | Full-screen pages such as login, dashboard, and farmer list |
| `lib/widgets/` | Reusable user interface components such as buttons, cards, and input fields |
| `lib/models/` | Data classes representing objects like farmers, assignments, and reports |
| `lib/services/` | Functions for API communication, authentication, and data caching |
| `lib/utils/` | Helper functions and constants |
| `test/` | Automated tests |

---

## Architecture Layers

The mobile application is organized into layers that separate different responsibilities.

### Presentation Layer

The presentation layer contains the screens and widgets that users see and interact with. Screens are full-page views such as the login screen, the dashboard, and the report creation screen. Widgets are reusable components such as buttons, text fields, and lists. This layer handles user input, displays data, and shows feedback messages.

### Business Logic Layer

The business logic layer contains the code that processes user actions and manages application state. It decides what happens when a user taps a button, submits a form, or navigates to a new screen. It validates input, formats data, and coordinates between the presentation layer and the data layer.

### Data Layer

The data layer handles communication with the backend API and local storage. It sends HTTP requests to fetch or save data. It stores data locally using Flutter's local storage options so the app can work offline. It synchronizes local data with the backend when a network connection is available.

---

## Core Components

The mobile application provides the following core components and screens.

### Welcome Screen

The welcome screen introduces users to the IvhuRedu mobile application. It displays the brand logo and provides options to log in or register.

### Authentication Screens

| Screen | Purpose |
|---|---|
| Login | Registered workers enter credentials to access the app |
| Registration | New users provide information to create an account |
| Verification | Supports account verification through the verification workflow |
| Password recovery | Allows users to reset their password when locked out |

### Dashboard or Home Screen

The dashboard provides access to the main mobile application functionality. It shows a summary of the worker's assignments, recent activities, and quick actions. It serves as the central hub for navigating to other parts of the app.

### Farmer Management Screens

| Screen | Purpose |
|---|---|
| Farmer list | View all registered farmers |
| Add farmer | Create a new farmer record with required information |
| Farmer details | View information about a registered farmer |

The app provides confirmation after successfully adding a farmer.

### Assignment Screens

| Screen | Purpose |
|---|---|
| Assignments list | View activities assigned to the worker |
| Assignment details | View information about a specific assigned activity |

Assignments support workers in organizing their field activities. Workers can use assignment information to carry out visits involving farmers and field locations.

### Reporting Screens

| Screen | Purpose |
|---|---|
| Report list | View available reports |
| Create report | Create a new report based on field activities |

Reports capture relevant information collected during field activities. Workers can submit completed reports through the application. The app provides confirmation after a report has been successfully submitted.

### Field Data Collection

During field visits, workers can access and record relevant information about farmers. They can capture information related to their field activities. The application supports collecting field images where required. Location information can be associated with field activities where supported by the device.

### Profile and Account Screen

The profile screen provides access to user account functionality. Workers can view and update their personal information, change settings, and log out of the application.

### Navigation

The application provides navigation between its main areas. The bottom navigation bar or drawer menu typically includes links to the Dashboard, Farmers, Assignments, Reports, and Profile sections.

---

## Security Measures

The mobile application implements several security measures to protect data and user accounts.

### Authentication

Protected functionality requires authenticated access. Users must log in before they can view assignments, create reports, or access farmer data. The app stores the authentication token securely and sends it with every API request.

### Role-Based Access

Application functionality is provided according to the user's role and permissions. Extension Workers cannot access supervisor or administrator features. The backend enforces these rules, and the app reflects them in the user interface.

### Sensitive Information

API keys, credentials, and other sensitive configuration values are not stored directly in the application source code. They are managed through environment variables or secure configuration files that are not committed to version control.

### Environment Configuration

Sensitive configuration is managed through environment variables or secure configuration files. Different configurations are used for development, staging, and production builds.

### Data Protection

Data stored locally on the device is protected. The app does not store passwords in plain text. Tokens are stored using secure storage mechanisms provided by the operating system.

---

## Installation

Follow these steps to set up the mobile application for development.

1. Install the Flutter SDK on your computer. Follow the official Flutter installation guide for your operating system.

2. Install Android Studio and the Android SDK for running the app on an emulator or physical device.

3. Clone the repository from GitHub and navigate to the mobile directory.

4. Install the project dependencies using Flutter's package manager.

5. Configure the environment variables or configuration files to point to the correct backend API URL.

6. Run the application on an emulator or connected device.

Useful commands during development include running tests with `flutter test` and checking code quality with `flutter analyze`.

---

## Troubleshooting

This section describes common problems and their solutions.

### Build Errors

If the app fails to build, ensure that the Flutter SDK is correctly installed and that the `flutter` command is available in your system path. Run `flutter doctor` to check for missing dependencies or configuration issues.

### API Connection Errors

If the app cannot connect to the backend, verify that the backend API URL in the configuration is correct and that the backend server is running. Check that your device or emulator has network access. If using a local backend, ensure the device and backend are on the same network.

### Authentication Failures

If login fails, verify that the credentials are correct. Check that the backend is running and accessible. Ensure the authentication token is being stored and sent correctly.

### Offline Sync Issues

If data created offline does not sync when connectivity returns, check that the sync logic is running. Verify that the backend is accessible. Check the local storage for any errors.

### Image Upload Failures

If images fail to upload, check that the device camera and storage permissions are granted. Verify that the image file is not corrupted and that the backend image upload endpoint is working.

### Location Services Not Working

If GPS coordinates cannot be recorded, ensure that location permissions are granted in the device settings. Check that the device GPS is enabled and that the app has permission to access location data.