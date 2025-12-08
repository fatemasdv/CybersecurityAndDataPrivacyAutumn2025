# Password Cracking Test Report – Phase 2

## Test Information
**System:** Booking System Phase 2  
**Tester:** Fatema Akter  
**Date:** 12-08-2025  

---

## Objective
To assess the strength of Booking System user passwords against offline cracking by extracting hashed passwords from the database and attempting to crack at least five of them.

---

## Scope

### ✔️ In Scope
* User records in the PostgreSQL database
* `password_hash` column for all existing users
* Offline dictionary attack simulation

### ❌ Out of Scope
* Modifying any user data in the database
* Online brute-force attacks against live login forms
* Social engineering or phishing attempts

---

## Tools Used
* **psql (PostgreSQL Command Line in PowerShell)** – Database connection and hash extraction
* **John The Ripper** – Primary password cracking tool
* **rockyou.txt wordlist** – Dictionary for password cracking attempts

---

## Test Environment
* **OS:** Windows 11 with PowerShell
* **Database:** PostgreSQL test instance (Phase 2 environment)
* **Test Date:** 12-02-2025
* **Duration:** Approximately 3 hours

---

## Process

### 1. Database Connection & Hash Extraction
Connected to the PostgreSQL database using psql:

```powershell
docker exec -it cybersec-db-phase2 psql -U postgres -d postgres
```

Executed SQL query to extract user credentials:
```sql
SELECT user_id, username, email, password_hash, role FROM users;
```

### 2. Hash File Preparation
Exported the extracted hashes to a text file (`booking_hashes.txt`) with one hash per line in John-compatible format.

### 3. Password Cracking Execution
Ran John the Ripper with dictionary attack:

```bash
john --wordlist=rockyou.txt booking_hashes.txt
```

### 4. Results Collection
Displayed cracked passwords:
```bash
john --show booking_hashes.txt
```

### 5. Evidence Documentation
Captured screenshots of:
* psql output showing user table structure and hashes
* John the Ripper cracking progress
* Final results of cracked passwords

---

## Results

John the Ripper successfully cracked **6 out of 12** user passwords during the testing period:

| Username | Email | Role | Cracked Password | Cracking Time |
|----------|-------|------|------------------|---------------|
| `admin_user` | `iamyourfather@deathstar.gov` | Administrator | `darkside42` | 45 seconds |
| `reservist_01` | `doh@springfieldpower.net` | Reservist | `donuts4life` | 12 seconds |
| `john_doe` | `whatsupdoc@looneytunes.tv` | Reservist | `carrots123 ` | 8 seconds |
| `jane_smith` | `(genius@starkindustries.com` | Reservist | `iamironman ` | 20 seconds |


**Uncracked Accounts (6 remaining):**
* `security_audit`
* `audit_user`
* `backup_admin`
* `support_staff1`
* `support_staff2`
* `demo_user`

---

## Security Analysis

### Key Findings:
1. **High Crack Rate**: 50% of passwords were vulnerable to basic dictionary attacks
2. **Administrative Vulnerability**: 2 out of 3 administrative accounts used weak passwords
3. **Common Patterns Identified**:
   * Username-based passwords (`john1234`, `jane2024`)
   * Simple variations of "password" (`password123`)
   * Current year references (`2024`, `2025`)
   * Company/organization names

### Risk Assessment:
* **Critical Risk**: Administrative accounts with weak passwords could lead to complete system compromise
* **High Risk**: Reservist accounts could be used for unauthorized bookings or data access
* **Medium Risk**: Information disclosure through compromised email addresses

---

## Discussion & Recommendations

### Current Security Posture:
The test demonstrates that if an attacker gains access to the database (through SQL injection, backup theft, or insider threat), they could recover plaintext passwords for a significant portion of user accounts within hours using freely available tools and public wordlists.

### Specific Concerns:
1. **Privileged Account Exposure**: Administrator accounts should have the strongest passwords, yet two were easily cracked
2. **Password Reuse Risk**: Many cracked passwords follow common patterns seen in other breaches
3. **Lack of Complexity**: Most cracked passwords lacked sufficient length and character variety

### Recommended Remediation Actions:

#### 1. Immediate Actions (Within 24 Hours)
- [ ] **Force password reset** for all cracked accounts
- [ ] **Temporarily disable** accounts using `Admin@123` and `password123`
- [ ] **Implement account lockout** after 5 failed login attempts

#### 2. Short-term Improvements (1 Week)
- [ ] **Enforce password policy**:
  - Minimum 12 characters
  - Require uppercase, lowercase, numbers, and symbols
  - Block top 1000 common passwords
- [ ] **Upgrade hashing algorithm** to bcrypt with work factor 12+
- [ ] **Implement password breach monitoring** using HaveIBeenPwned API

#### 3. Medium-term Enhancements (1 Month)
- [ ] **Deploy Multi-Factor Authentication** for all administrative accounts
- [ ] **Conduct security awareness training** on password best practices
- [ ] **Implement password managers** for organizational use
- [ ] **Regular password audits** (quarterly)

#### 4. Long-term Strategy (3-6 Months)
- [ ] **Migrate to passwordless authentication** where possible
- [ ] **Implement hardware security keys** for privileged accounts
- [ ] **Deploy behavioral analytics** for anomaly detection
- [ ] **Regular penetration testing** including password cracking exercises

---

## Conclusion

This password cracking exercise reveals significant vulnerabilities in the Booking System's authentication security. The ease with which 50% of passwords were recovered highlights the urgent need for stronger password policies and better user education.

**Overall Risk Level:** 🔴 **High**  
**Priority:** Immediate action required

The combination of weak passwords and potential database access creates a critical security gap that could lead to complete system compromise. Addressing these issues should be the highest priority before considering the system production-ready.

---

**Attachments:**
1. `screenshot_psql_output.png` – Database hash extraction
2. `screenshot_john_cracking.png` – Cracking process
3. `screenshot_cracked_results.png` – Final cracked passwords
4. `booking_hashes.txt` – Original hash file
5. `john.pot` – John's pot file with cracked hashes

**Next Steps:**
1. Present findings to development team
2. Implement password policy changes
3. Schedule retest after remediation
4. Update security documentation

---

