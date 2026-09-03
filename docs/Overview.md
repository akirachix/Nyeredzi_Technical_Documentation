# 1. Overview

This page introduces the IvhuRedu platform. It explains what the product is, the problem it solves, who uses it, what it can do, and how the main workflow operates from start to finish.

---

## Product Definition

IvhuRedu is a digital agricultural platform that connects farmers and Agritex officers. The name IvhuRedu translates to "Our Soil." The platform improves how agricultural information is collected, shared, managed, and monitored in the field.

The system provides three main ways to access its services:

| User Type | Access Method | Device |
|---|---|---|
| Farmers | USSD | Basic mobile phone |
| Extension Workers | Flutter mobile app | Android smartphone |
| Supervisors and Administrators | Next.js web dashboard | Computer |

All three access channels connect through a single backend API and a centralized PostgreSQL database. This design ensures that data remains consistent no matter which channel a user chooses.

---

## Problem Statement

Before IvhuRedu, agricultural extension services faced several challenges that reduced their effectiveness.

**Farmers in rural areas often lack smartphones and reliable internet access.** This made it difficult for them to report problems, request assistance, or share information about their land using digital tools. Many farmers could not participate in digital agricultural programs at all.

**Agritex Extension Workers spent much of their time managing paper records.** Field reports were handwritten, photographs were stored separately, and location data was often inaccurate or missing. This made it hard to organize information and track progress over time.

**Supervisors and Extension Officers lacked a centralized view of field activities.** They could not easily see which workers were available, which farmers needed help, or what issues were being reported across different wards. Decision-making was slowed by the lack of timely, accurate data.

**Communication between farmers and officers relied on phone calls and physical visits.** There was no reliable way to send alerts, confirm assignments, or notify farmers when help was on the way.

IvhuRedu was built to solve these problems by providing accessible digital tools for every participant in the agricultural extension process.

---

## Target Users

The platform serves four distinct groups of users. Each group has different needs, devices, and permissions.

### Farmers

Farmers are the primary beneficiaries of the platform. They interact with IvhuRedu through USSD using any basic mobile phone. They do not need a smartphone or internet connection. Farmers can request assistance, report land degradation, ask for advice, and receive SMS alerts about their requests.

### Agritex Extension Workers

Extension Workers are field officers who visit farms and provide direct support to farmers. They use the Flutter mobile application on Android smartphones. Through the app, they view their assignments, access farmer information, submit field reports, collect images, and record GPS coordinates. They work offline when network coverage is poor and sync data when connectivity returns.

### Agritex Extension Officers and Supervisors

Supervisors manage Extension Workers and oversee field operations. They use the Next.js web dashboard on desktop or laptop computers. Supervisors can register workers, assign tasks, monitor reports, view analytics, send SMS alerts, and manage farmer requests. They have a comprehensive view of all platform activities.

### Administrators

Administrators are the top-level system managers. They also use the web dashboard. Administrators can register and manage supervisors, view all users, monitor system activity, and configure platform settings. They have the highest level of access to the system.

---

## Key Features

IvhuRedu provides a comprehensive set of features designed to support every stage of agricultural extension work.

### Farmer USSD Access

Farmers access the platform through USSD sessions powered by Africa's Talking. The interface presents simple text menus that work on any mobile phone. Farmers can navigate using number inputs to request help, report problems, or check status. The system sends SMS confirmations and updates to keep farmers informed.

### Agritex Extension Worker Mobile App

The mobile application provides tools for field work. Workers can view lists of registered farmers and add new farmer records. They see their assignments with details about what task to perform and where. They create field reports that include text descriptions, photographs, and location data. The app works offline and synchronizes with the backend when a network connection is available.

### Assistance and Reporting

Farmers request assistance through USSD. Extension Workers submit field reports through the mobile app. Supervisors monitor all requests and reports through the web dashboard. Reports include evidence such as field images and GPS coordinates. Request statuses track progress from pending through in progress to resolved.

### Agritex Web Dashboard

The dashboard provides centralized monitoring and management. Supervisors can manage Extension Workers, view analytics about registered farmers and field visits, handle incoming farmer requests, send individual or broadcast SMS messages, and configure their account settings. The dashboard presents data through tables, charts, and maps.

### Location and Assignment Management

The platform collects and manages location information for farmers, field activities, and Extension Workers. The Location Module connects to LocationIQ for geocoding and address lookup. Supervisors assign requests to workers based on location, availability, and workload. The system tracks assignment progress and worker status.

### SMS Alerts and Notifications

The SMS Alert Module sends text messages to farmers and workers at key stages of the workflow. Messages include request confirmations, assignment notifications, status updates, and broadcast alerts. The platform integrates with Africa's Talking and SMS Leopard for message delivery.

### Secure Authentication and Data Management

The platform uses role-based access control. Each user role sees only the features and data appropriate to their responsibilities. Authentication uses secure tokens. Passwords and PINs are encrypted. Sensitive configuration values are stored in environment variables and never committed to source control.

---

## Workflow Overview

The following steps describe how information flows through the platform from a farmer's initial request to the final resolution and reporting.

1. A farmer dials the USSD code on their basic mobile phone and navigates the menu to request assistance or report a land problem.

2. Africa's Talking receives the USSD input and forwards it to the IvhuRedu backend API as a structured request.

3. The backend stores the farmer request in the PostgreSQL database through the Assistance Module.

4. The request becomes visible on the web dashboard in the Farmer Request section. A supervisor sees the new pending request.

5. The supervisor reviews the farmer details and location information. They select an available Extension Worker and assign the request through the Assignment Module.

6. The assigned worker receives the assignment in their Flutter mobile application. They may also receive an SMS alert about the new task.

7. The Extension Worker travels to the farmer's location. They use the mobile app to access the farmer's information and navigate to the site.

8. During the visit, the worker assesses the situation, takes photographs of the field, and records the GPS coordinates.

9. The worker creates a field report in the mobile app. The report includes observations, images, and location data. They submit the report to the backend.

10. The backend stores the report in the database through the Reporting Module. The images are saved and linked to the report.

11. The completed report appears on the web dashboard. The supervisor can review the evidence and monitor the resolution.

12. The farmer may receive an SMS notification that their request has been addressed and the report has been filed.

13. Throughout this process, analytics data is collected and displayed on the dashboard, showing metrics such as total registered farmers, field visits, disease and pest reports, crop distribution, and worker engagement.

This workflow demonstrates how the USSD service, mobile application, and web dashboard all connect through the backend API to create a seamless experience for every user.