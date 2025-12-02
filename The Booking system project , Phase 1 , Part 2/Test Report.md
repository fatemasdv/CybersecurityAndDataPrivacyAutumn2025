








ZAP Test Report for Booking System
Tester:
•	Name:   Fatema Akter
•	Date:12-2-2025














Purpose:
The report is intended to show what beans were collected after the booking system application has been running for some days. The goal here is to hunt vulnerabilities (such as SQL Injection, Path Traversal, Cross-Site Request Forgery (CSRF)) using OWASP ZAP on them.
Scope:
•	Tested Components:
1.User Registration functionality
2.User Login functionality
3.The public endpoints (like homepage, booking system)

•	Excluded Components: None
•	Test Approach:
OWASP ZAP (Zed Attack Proxy) was used to perform Black-box testing for security on the application’s external pages, including form submission

Test Environment & Dates:
Test Environment:
•	OS: Linux
•	Web Server: Apache
•	Database: PostgreSQL
•	Start Date: 2025-12-02
•	End Date: 2025-12-02
Start Date: 2025-12-02
End Date: 2025-12-02




Assumptions & Constraints:
1.The project was available to test between these dates.
2.Tests are run on the most recent released version of the app.

Executive Summary
Overview of Findings:
The security audits of the app uncovered numerous flaws with implications ranging from the minor to those compromising safety. Most severe are the findings for Path Traversal and SQL Injection exposing your system to unauthorized access and data manipulation. There were also medium-risk vulnerabilities due to absent Anti-CSRF tokens and security headers.
Key Findings:
Path Traversal (High Risk) – The application was vulnerable to Path Traversal, meaning a malicious user has the ability to access files outside of the application root.
SQL Injection (High Risk) – The application is susceptible to SQL injection; the attacker can execute their own SQL queries.
No Anti-CSRF Tokens (Medium Risk) – It leaves forms' action vulnerable to CSRF attacks, which can lead to users' actions being taken without their consent.
No Content Security Policy (CSP) Header (medium risk): Missing CSP header(s) could make the website vulnerable to cross-site scripting and other script-based attacks.
Lacks Anti-clickjacking Header (Medium Risk) – This site can be used in clickjacking attacks because it does not contain enough protection against the use of their pages within a frame.








Risk Assessment:
Risk Level
	Number of alerts	Recommendation

High
	2
	Immediate remediation required

Medium
	4	Urgent fix to lower the risk of potential exploitation.

Low
	1	observe and correct in next releases

Informational
	1	No threat can be reviewed later

		
		
		
		
		
Overall Risk Level: High











Findings Summary
ID	Severity	Finding	Description	Evidence / Proof
F-01	 High	SQL Injection	An attacker might be able to transversing out of an application's root directory and access some files.
	POST /register input manipulation
F-02	High	Absence of Anti-CSRF Tokens	The application is prone to an SQL-injection issue because it fails to sufficiently sanitize user-supplied data submitted by the username field. An attacker may manipulate this input to carry out arbitrary queries in the underlying database.
	HTTP/1.1 500 Internal Server Error
F-03	Medium	Content Security Policy Header	Absence of Anti-CSRF Tokens
No Anti-CSRF tokens are included in the HTML forms for posting requests, this exposes the application to CSRF attacks.
	<form action="/register" method="POST">
F-04	Medium	Missing Anti-clickjacking Header	No CSP header is set, making the site vulnerable to XSS and injections.
	Evidence: Missing CSP Header
F-05	Low	SQL Injection	The app does not have an X-Frame-Options or Content-Security-Policy header defending against clickjacking.
	No X-Frame-Options header


