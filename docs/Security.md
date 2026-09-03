# 6. Security

This page describes the security posture of the IvhuRedu platform. It covers how data is classified, how the network is protected, how the application is secured, how data is protected, how webhooks are secured, physical security considerations, how incidents are handled, how risks are managed, and a summary of the security approach.

---

## Security Posture

IvhuRedu follows a defense-in-depth approach to security. This means security is built into multiple layers of the system rather than relying on a single measure. The platform protects data in transit, at rest, and during processing. Access is controlled based on user roles. Sensitive values are never exposed in source code. All inputs are validated before processing.

The platform handles sensitive agricultural and personal data. This includes farmer contact information, location coordinates, field reports, and user credentials. Protecting this data is essential for maintaining trust with users and complying with data protection principles.

---

## Data Classification

Platform data is classified into four categories based on sensitivity.

| Category | Description | Examples |
|---|---|---|
| Public | Can be shared openly without risk | Informational website content, public brand materials, general platform descriptions |
| Internal | Meant for platform users, no sensitive personal information | Anonymized analytics, general reports, operational metrics |
| Sensitive | Personal information that must be protected | Farmer names, phone numbers, addresses, location coordinates, user account details, assignment records |
| Restricted | Most sensitive information, highest protection | Passwords, PINs, API keys, database credentials, authentication tokens |

Access to sensitive data is restricted to authenticated users with appropriate roles. Restricted data is encrypted, stored in environment variables, and never committed to source control. Only system administrators should have access to restricted data.

---

## Network Security

Network security protects data as it travels between users and the platform.

### HTTPS

All communication between users and the platform uses HTTPS. This encrypts data in transit so it cannot be read or modified by attackers. The backend API, web dashboard, and mobile app all require HTTPS connections.

### CORS

Cross-Origin Resource Sharing is configured on the backend to restrict which websites can make requests to the API. Only approved origins such as the web dashboard domain are allowed. This prevents malicious websites from making unauthorized API calls.

### Trusted Hosts

The backend uses TrustedHostMiddleware to ensure that requests come from expected hostnames. This prevents host header attacks where a malicious client sends a fake host header to bypass security.

---

## Application Security

Application security protects the platform from attacks that target the code and logic.

### Input Validation

All user input is validated before processing. The backend uses Pydantic models to define the expected shape of incoming data. If the data does not match the expected format, the API rejects the request with a 400 error. This prevents injection attacks and malformed data from reaching the database.

### Authentication

Users must authenticate before accessing protected features. The backend issues JWT access tokens after successful login. These tokens expire after a short time. Refresh tokens allow users to obtain new access tokens without logging in again. Tokens are sent in the Authorization header of API requests.

### Authorization

Role-based access control enforces permissions at the API level. Each endpoint checks the user's role before processing the request.

| Role | Access Level |
|---|---|
| Farmer | USSD menus only |
| Extension Worker | Mobile app features only |
| Supervisor | Dashboard features within their scope |
| Administrator | Full system access |

### Password Security

Passwords are hashed using a strong one-way hashing algorithm before storage. When a user logs in, the backend hashes the submitted password and compares it to the stored hash. Passwords are never stored in plain text. Users can change their passwords through the authenticated change-password endpoint.

### Session Management

Sessions expire after a period of inactivity. Users must log in again when their session expires. The dashboard and mobile app handle session expiry by redirecting to the login screen or refreshing the token.

---

## Data Security

Data security protects information stored in the database and configuration files.

### Encryption at Rest

Sensitive data such as passwords, PINs, and tokens are encrypted before being stored in the PostgreSQL database. This ensures that even if the database is accessed without authorization, the sensitive values cannot be read.

### Environment Variables

All API keys, database URLs, and secrets are stored in environment variables. These values are never written into the source code. The `.env` files containing these values are excluded from version control using `.gitignore`. In production, environment variables are managed through the Heroku dashboard.

### Database Security

The PostgreSQL database is protected by credentials that are only known to the backend application. Direct database access is restricted. The database runs on a managed service that handles patching, backups, and network isolation.

### Backup and Recovery

The database is backed up regularly. In the event of data loss or corruption, backups can be restored. The backup strategy ensures that critical agricultural data is not permanently lost.

---

## Webhook Security

Webhooks are HTTP callbacks sent by external services to the backend. The platform receives webhooks from Africa's Talking for USSD sessions and SMS delivery status.

### Webhook Verification

Webhook endpoints should verify that requests come from the expected sender. This can be done using signature verification or IP allowlisting where supported by the external service.

### HTTPS Webhooks

Webhook URLs must use HTTPS. Africa's Talking requires publicly reachable HTTPS endpoints. For local development, a tunneling service such as ngrok can provide a temporary public URL.

### Input Sanitization

Webhook endpoints validate and sanitize incoming data before processing. They check that required fields are present and that values are within expected ranges.

---

## Physical Security

Physical security is handled by the hosting providers.

### Heroku Infrastructure

The backend runs on Heroku, a managed platform. Heroku handles the physical security of the servers, network equipment, and data centers. This includes access controls, surveillance, and environmental protections.

### Device Security

Extension Workers are responsible for the physical security of their smartphones. Lost or stolen devices should be reported so that accounts can be deactivated. The mobile app stores authentication tokens securely, but physical access to an unlocked device could allow unauthorized use.

---

## Incident Response

An incident is any event that compromises the security or availability of the platform.

### Detection

Incidents are detected through monitoring and logging. The backend logs errors and important events. Failed authentication attempts, unusual API activity, and external service failures are logged for investigation.

### Response

When an incident is detected, the team investigates the cause and scope. Affected accounts may be suspended. Vulnerabilities are patched. If data is compromised, affected users are notified.

### Recovery

After an incident is contained, the team restores normal operations. This may involve rolling back to a previous stable release, restoring from backups, or reconfiguring security settings.

### Documentation

All incidents are documented. The documentation includes what happened, how it was discovered, what actions were taken, and what measures were implemented to prevent recurrence.

---

## Risk Management

Risk management is the ongoing process of identifying and reducing security risks.

### Dependency Management

Third-party libraries and frameworks are kept up to date. Security patches are applied promptly. Outdated dependencies are reviewed regularly.

### Access Reviews

User access is reviewed periodically. Accounts for users who no longer need access are deactivated. Role assignments are checked to ensure they match current responsibilities.

### Code Review

All code changes are reviewed before merging. Reviewers check for security issues such as exposed secrets, missing input validation, and improper access control.

### Penetration Testing

The platform may undergo penetration testing to identify vulnerabilities that automated tools cannot find. Findings are addressed before they can be exploited.

---

## Conclusion

Security is an ongoing priority for IvhuRedu. The platform implements multiple layers of protection including network encryption, input validation, role-based access control, password hashing, and secure configuration management. All team members share responsibility for maintaining security by following the code standards, protecting credentials, and reporting suspicious activity.