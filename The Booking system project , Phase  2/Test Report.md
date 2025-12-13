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

## 3. Tools and Environment

### Tools Used

#### psql (PostgreSQL command-line client)
- Used from PowerShell to connect to the PostgreSQL instance and extract user data.

#### John the Ripper
- Primary password cracking tool for initial dictionary-based tests.

#### rockyou.txt wordlist
- Public, widely used password dictionary.

#### Hashcat 
- Used for more advanced rules-based attacks (`rockyou-30000.rule`) to further evaluate crackability of MD5 hashes.

Used for more advanced rules-based attacks (rockyou-30000.rule) to further evaluate crackability of MD5 hashes.
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
SELECT * FROM  bookking_users;
```

### 2. Hash File Preparation
Exported the extracted hashes to a text file (`booking_hashes.txt`) with one hash per line in John-compatible format.
### 3 Password Cracking Execution
### 3.1 Password Cracking Execution johe the ripper
Ran John the Ripper with dictionary attack:

```bash
john --wordlist=rockyou.txt booking_hashes.txt
```
### 3.2 Cracking by Hashcat 

To further evaluate how easily these hashes could be cracked by an attacker using more advanced tooling:

- Prepared a Hashcat input file containing `email:hash` pairs  
  - Example: `whysoserious@gothamchaos.net:f158d4...`

#### Initial Dictionary Attack (Baseline)
Ran a straight dictionary attack (no additional hits beyond what John the Ripper already found):

```bash
hashcat -m 0 -a 0 --username /home/kali/hashpass.txt /usr/share/wordlists/rockyou.txt

### 4. Results Collection
Displayed cracked passwords:
```bash
john --show userhashes.txt
```

### 5. Evidence Documentation
Captured screenshots of:
* psql output showing user table structure and hashes
* John the Ripper cracking progress
* Final results of cracked passwords

---

## Results

John the Ripper successfully cracked **6 out of 12** user passwords during the testing period:

| user_id | username                   | password_hash                      | role          | birthdate  | user_token                             |
|--------:|----------------------------|------------------------------------|---------------|------------|----------------------------------------|
| 1       | whatsupdoc@looneytunes.tv  | a0e8402fe185455606a2ae870dcbc4cd   | reserver      | 1980-04-12 | b7a8d729-f5c3-4f5a-86e2-9cdb73511ad9   |
| 2       | doh@springfieldpower.net   | d730fc82effd704296b5bbcff45f323e   | administrator | 1975-05-10 | f3b93c24-8b55-4a0d-8b3c-97c4b8a1e728   |
| 3       | darkknight@gothamwatch.org | 735f7f5e652d7697723893e1a5c04d90   | reserver      | 1988-09-15 | 94e30d50-4b2e-47b4-920a-0c5f6721a5a2   |
| 4       | chimichanga@fourthwall.com | 7cb56c2b86150b797cff32eaef97f338   | administrator | 1991-02-22 | de3d09e1-fc3a-4938-80c6-bef1b45b91b2   |
| 5       | iamyourfather@deathstar.gov| 706ab9fc256efabf4cb4cf9d31ddc8eb   | reserver      | 1960-06-01 | c02dd33f-198a-43e7-882f-b4a73b5dbf18   |
| 6       | elementary@221bbaker.uk    | 12c9cef0bfb6b91c42b363b4cf02d8bb   | administrator | 1982-01-07 | 9c6ffbe1-44eb-4428-b3fd-bcc44f38de31   |
| 7       | genius@starkindustries.com | d50ba4dd3fe42e17e9faa9ec29f89708   | reserver      | 1970-05-29 | af9c8d38-9d8f-4b71-9b48-e67212a6355a   |
| 8       | whysoserious@gothamchaos.net| f158d479ee181aac68b000a60e7a3d7a  | administrator | 1985-07-18 | dd0b5c4b-1e99-4193-98c8-317f48b4b6f6   |
| 9       | quackattack@duckburg.org   | ea261222d4867b3ebdfadbe2b35e19d5   | reserver      | 1992-11-25 | 4f5a3ef5-191e-4de0-a68e-53e349e6788b   |
| 10      | ruhroh@mysterymachine.com  | ad17fbd845000b11678ccbf94e135b56   | reserver      | 1987-03-30 | fb9d315b-d1f1-49a1-8717-f28db6b94989   |
| 11      | foo-bar@example.com        | 1b12f2c371d1e578f4a61d3a3e6c49b6   | administrator | 2025-12-08 | e67f82ff-8259-49a8-8074-4584e117b793   |
| 12      | mun@ac.io                  | 0343f6458ad05994f532135e75083733   | reserver      | 1999-11-11 | c9b3ca4b-1074-4956-906c-c0fb364bff50   |

----
| User / Email                                                        | Password    |
| ------------------------------------------------------------------- | ----------- |
| [whatsupdoc@looneytunes.tv](mailto:whatsupdoc@looneytunes.tv)       | carrots123  |
| [doh@springfieldpower.net](mailto:doh@springfieldpower.net)         | donuts4life |
| [iamyourfather@deathstar.gov](mailto:iamyourfather@deathstar.gov)   | darkside4e2 |
| [genius@starkindustries.com](mailto:genius@starkindustries.com)     | iamironman  |
| [whysoserious@gothamchaos.net](mailto:whysoserious@gothamchaos.net) | chaos123!   |


---

## Security Analysis

### Key Findings:
1. **High Crack Rate**: 50% of passwords were vulnerable to basic dictionary attacks
2. **Administrative Vulnerability**: 2 out of 3 administrative accounts used weak passwords
3. **Common Patterns Identified**:
   * Username-based passwords 
   * Simple variations of 
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
4. `booking_hashes.png` userhashes.txt – Original hash file




---












