# Security Policy

## Supported Versions

We actively support the following versions of GigSync with security updates:

| Version | Supported          | End of Support |
| ------- | ------------------ | -------------- |
| 1.0.x   | :white_check_mark: | TBD            |
| < 1.0   | :x:                | Immediately    |

## Reporting a Vulnerability

The GigSync team takes security bugs seriously. We appreciate your efforts to responsibly disclose your findings, and will make every effort to acknowledge your contributions.

### How to Report a Security Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them via email to:
- **Email**: [security@gigsync.app](mailto:security@gigsync.app) or [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com)
- **Subject**: `[SECURITY] Vulnerability Report`

Include the following information in your report:

1. **Type of issue** (e.g., SQL injection, XSS, authentication bypass)
2. **Full paths of source file(s)** related to the manifestation of the issue
3. **Location of the affected source code** (tag/branch/commit or direct URL)
4. **Step-by-step instructions** to reproduce the issue
5. **Proof-of-concept or exploit code** (if possible)
6. **Impact of the issue**, including how an attacker might exploit the issue

### What to Expect

1. **Acknowledgment**: We'll acknowledge receipt of your vulnerability report within 24-48 hours.

2. **Initial Assessment**: We'll provide an initial assessment of the report within 72 hours, including whether we consider it a valid security issue.

3. **Progress Updates**: We'll keep you informed about our progress resolving the issue.

4. **Resolution**: We aim to resolve critical security issues within 7-14 days, depending on complexity.

5. **Disclosure**: Once the issue is resolved, we'll work with you on an appropriate disclosure timeline.

### Scope

This security policy applies to:

- **GigSync Web Application**: Main web platform
- **GigSync API**: All API endpoints
- **GigSync Mobile Apps**: iOS and Android applications
- **Infrastructure**: Deployment and hosting components

### Out of Scope

The following are generally out of scope for our security policy:

- Issues in third-party libraries without demonstrable impact on GigSync
- Social engineering attacks
- Physical attacks
- Denial of service attacks
- Issues requiring physical access to user devices
- Issues in browsers or other third-party software
- Content injection without demonstrable security impact

### Security Measures

GigSync implements the following security measures:

#### Authentication & Authorization
- Secure session management
- Multi-factor authentication support
- Role-based access control (RBAC)
- JWT token security with proper expiration

#### Data Protection
- Encryption at rest and in transit (TLS 1.3)
- Secure database configurations
- PII data protection and anonymization
- GDPR compliance measures

#### Application Security
- Input validation and sanitization
- CSRF protection
- XSS prevention
- SQL injection prevention
- Security headers implementation

#### Infrastructure Security
- Regular security updates and patches
- Secure CI/CD pipelines
- Environment variable protection
- Regular security audits

#### Payment Security
- PCI DSS compliance through Stripe
- Secure payment processing
- No storage of sensitive payment data
- Fraud detection and prevention

### Responsible Disclosure Guidelines

We believe in responsible disclosure and ask that security researchers:

1. **Give us a reasonable time** to investigate and mitigate an issue before making any information public
2. **Do not access** a user's data or modify/delete data
3. **Do not perform** attacks that could harm the reliability/integrity of our services
4. **Do not use** social engineering, physical, or denial of service attacks
5. **Do not violate** any laws or regulations

### Recognition

We believe in recognizing security researchers who help us maintain the security of GigSync:

- **Public Recognition**: With your permission, we'll publicly acknowledge your contribution
- **Hall of Fame**: Security researchers will be listed in our security acknowledgments
- **Swag**: We may send GigSync merchandise as a token of appreciation
- **Bounty Program**: We're planning to implement a bug bounty program in the future

### Security Acknowledgments

We'd like to thank the following security researchers for their responsible disclosure:

<!-- This section will be updated as we receive reports -->
- No reports received yet

### Contact Information

For security-related questions or concerns:

- **Email**: [security@gigsync.app](mailto:security@gigsync.app)
- **General Contact**: [bejugammahaswi@gmail.com](mailto:bejugammahaswi@gmail.com)
- **GitHub**: [Security Advisory](https://github.com/Mahas1234/GigSync/security/advisories)

### Legal

This security policy is subject to our [Terms of Service](https://gigsync.app/terms) and [Privacy Policy](https://gigsync.app/privacy).

---

**Thank you for helping keep GigSync and our users safe! 🔒**