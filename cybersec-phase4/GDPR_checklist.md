# GDPR Compliance Checklist – Web-based Booking System

## Personal Data Mapping and Minimization

| Result | Checklist Item |
| :----: | :--- |
| ✅/❌/⚠️ | All personal data collected and processed in the system have been identified (e.g., name, email, age, username). |
| ✅/❌/⚠️ | Only necessary personal data is collected in accordance with the data minimization principle. |
| ✅/❌/⚠️ | User age is recorded to verify that the booker is over 15 years old. |

---

## User Registration and Management

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | The registration form includes GDPR-compliant consent for processing personal data (privacy policy acceptance). |
| ⚠️ | Users can view, edit, and delete their own personal data via their account. |
| ✅/❌/⚠️ | Administrators can delete a reserver in accordance with the “right to be forgotten”. |
| ✅/❌/⚠️ | Underage registration (under 15 years) and booking functionality are restricted. |

---

## Booking Visibility

| Result | Checklist Item |
| :----: | :--- |
| ✅ | Non-logged-in users can only see bookings at the resource level without personal data. |
| ⚠️ | Personal data such as names and emails are not exposed publicly or to unauthorized users. |

---

## Access Control and Authorization

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | Only administrators can add, modify, and delete resources and bookings. |
| ⚠️ | Role-based access control is implemented (e.g., reserver vs. administrator). |
| ✅/❌/⚠️ | Administrator privileges are limited to prevent unauthorized use of personal data. |

---

## Privacy by Design Principles

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | Privacy by Default is implemented, collecting minimal data by default. |
| ⚠️ | Logs are implemented without unnecessarily storing personal data. |
| ✅/❌/⚠️ | Forms and system components are designed with data protection in mind (secure login, minimal fields). |

---

## Data Security

| Result | Checklist Item |
| :----: | :--- |
| ✅/❌/⚠️ | Protection against CSRF, XSS, and SQL injection is implemented. |
| ✅/❌/⚠️ | Passwords are securely hashed using strong algorithms (e.g., bcrypt, Argon2). |
| ⚠️ | Data backup and recovery processes are GDPR-compliant. |
| ⚠️ | Personal data is stored in data centers located within the EU. |

---

## Data Anonymization and Pseudonymization

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | Personal data is anonymized where possible. |
| ✅/❌/⚠️ | Pseudonymization techniques are used to protect data while maintaining usability. |

---

## Data Subject Rights

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | Users can request or download all personal data related to them (right of access). |
| ⚠️ | Users can request deletion of their personal data (right to be forgotten). |
| ⚠️ | Users can withdraw their consent for data processing. |

---

## Documentation and Communication

| Result | Checklist Item |
| :----: | :--- |
| ⚠️ | A privacy policy is available during registration and easily accessible. |
| ⚠️ | Administrators and developers have documented data protection practices. |
| ❌ | A documented data breach response process is in place. |

---

## GDPR Compliance Summary

| Area | Summary |
|------|---------|
| Personal Data & Minimization | Data collection is mostly limited and identified, but enforcement needs improvement. |
| User Management | Consent and deletion mechanisms exist, but user self-service features need enhancement. |
| Security & Privacy | Core security measures are implemented; backups and data location need better compliance. |
| User Rights | Data access, deletion, and consent withdrawal are partially supported and mostly manual. |
| Documentation | Privacy policy exists, but internal documentation and breach response planning are lacking. |

---

## Overall Assessment

⚠️ **Partially GDPR Compliant**

The system meets several GDPR requirements but requires improvements in documentation, automation of user rights, access control enforcement, and a formal data breach response plan.
