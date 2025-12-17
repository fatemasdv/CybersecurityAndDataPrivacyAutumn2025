# Booking System Project – Phase 3 Report

**Course:** Cybersecurity and Data Privacy (Autumn 2025)  
**Focus:** Authorization, Logging, Security Testing, Shielding, GDPR  
**Environment:** Kali Linux, Docker, Docker Compose  
**Application:** Booking System – Phase 3

---
##Tester(s) Fatema Akter

## 1. Introduction

The objective of Phase 3 was to evaluate the **authorization mechanisms**, exposed functionality, and overall security posture of the Booking System application. The focus was on identifying accessible endpoints, validating role-based access control (RBAC), performing controlled security testing, and assessing compliance with security and privacy principles defined in the project specification.

---

## 2. Test Environment

The application was deployed locally using Docker Compose on Kali Linux.

* Web application exposed at:  
  **[http://localhost:8003](http://localhost:8003)**
* Containers:
  * `cybersec-web-phase3`
  * `cybersec-db-phase3`

Verification:

```bash
docker compose ps
```

---

## 3. Application Surface Discovery

### 3.1 Methodology

Endpoint discovery was performed using:

* Gobuster
* wfuzz / ffuf (cross‑validation)
* Manual browser navigation

Because the application returns **redirects instead of HTTP 404** for non‑existing paths, **response‑length filtering** was applied.

### 3.2 Gobuster Command

```bash
gobuster dir -u http://localhost:8003 \
  -w /usr/share/wordlists/dirb/common.txt \
  --exclude-length 0 -t 20 \
  -o gobuster_phase3.txt
```

### 3.3 Discovered Endpoints

The following endpoints were identified:

| Endpoint       | Description                  |
|----------------|------------------------------|
|      		 |				|
| `/register`    | User registration            |
| `/resources`   | Resource listing             |
| `/reservation` | Reservation handling         |


No `/admin` or `/api/admin` endpoints were discovered.

---

## 4. Authorization Testing

### 4.1 Roles Tested

* Guest (not logged in)
* Reserver (authenticated user)
* Administrator (not implemented in Phase 3)

---

### 4.2 Guest (Not Logged In)

**Allowed**

* Access `/`
* Access `/login`
* Access `/register`
* View `/resources`
* No personal data exposed

**Denied**

* `/reservation`
* `/profile`
* `/admin/*`
* `POST /api/reservations`
* `/api/*`

**Result:** ✔ Authorization enforced correctly

---

### 4.3 Reserver (Authenticated User)

**Allowed**

* Login / logout
* View `/resources`
* Create reservations
* View own profile and own reservations
* Access `/api/reservations` (own data only)

**Denied**

* `/admin`
* `/api/admin`
* Any admin‑only functionality
* Privilege escalation via parameter tampering

**Result:** ✔ Authorization enforced correctly

---

### 4.4 Administrator Role (Phase 3 Scope)

Administrative functionality is **not implemented or exposed** in Phase 3.

Evidence:

```bash
curl -i http://localhost:8003/admin        → 302 Not Found handler
curl -i http://localhost:8003/api/admin    → 404 {"error":"Not Found"}
```


---

## 5. Hidden Endpoint Discovery

### Tools Used

* Gobuster
* wfuzz
* ffuf
* OWASP ZAP

### Findings

* No hidden admin or management endpoints discovered
* No IDOR vulnerabilities detected
* No authorization bypasses identified

✔ Negative results confirmed across multiple tools

---

## 6. Security Testing (OWASP ZAP)

An authenticated OWASP ZAP scan was performed.

### Summary

* High risk: 0
* Medium risk: 0
* Low risk: 1 (ZAP version out of date)
* Informational: Authentication & session handling identified

### Interpretation

* No exploitable vulnerabilities were found
* Authentication and session management were correctly detected
* Findings are informational and do not require remediation

---

## 7. Shielding and Hardening

The application applies security headers consistently, including on error responses:

* `Content‑Security‑Policy`
* `X‑Frame‑Options: DENY`
* `X‑Content‑Type‑Options: nosniff`
* `Frame‑Ancestors 'none'`

These headers mitigate common web attacks such as XSS and clickjacking.

---

## 8. Logging Observations

Logs were reviewed via Docker:

```bash
docker compose logs web
```

Observations:

* Authentication and routing events logged
* No sensitive data exposed in logs
* Logging supports auditing and incident analysis

---

## 9. GDPR and Privacy Considerations

Observed GDPR‑aligned practices:

* Authentication protects personal data
* CSRF tokens implemented
* Session cookies used securely
* No unnecessary personal data exposure

Limitations:

* No user‑visible data export or deletion features
* Data retention policy not explicitly documented

---

## 10. Conclusion

Phase 3 objectives were successfully achieved. The Booking System demonstrates:

* Correct authorization enforcement
* Clear separation between public and protected functionality
* No hidden or privileged endpoints
* No high‑risk vulnerabilities

* Effective security headers and session handling
