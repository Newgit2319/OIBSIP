# CyberSecurity Task 3 – SQL Injection using DVWA

## Objective

Demonstrate a basic SQL Injection vulnerability using Damn Vulnerable Web Application (DVWA) in a controlled local Kali Linux environment.

## Lab Environment

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Web Application | DVWA |
| Web Server | Apache |
| Database | MariaDB |
| Target | Localhost (127.0.0.1) |
| Security Level | Low |
| Vulnerability | SQL Injection |

## SQL Injection Testing

### Normal Query

The following input was first tested:

```text
1
The application returned the user associated with ID 1.

This established the expected behavior before testing the vulnerability.

Injection Test

The following input was then tested:

1' OR '1'='1

The application returned multiple user records instead of a single record.

Observed records included:

admin
Gordon Brown
Hack Me
Pablo Picasso
Bob Smith

This demonstrates that the application's SQL query can be manipulated through user-controlled input.

Evidence

The evidence/ directory contains:

normal_query.txt – Normal SQL query test
injection_result.txt – Successful SQL injection result
sql_injection_explanation.txt – Explanation of the observed vulnerability

The screenshots/ directory contains visual evidence of the testing process.

Screenshots
1. 01-dvwa-sql-injection-low.png – DVWA SQL Injection module at Low security level
2. 02-normal-sql-query.png – Normal User ID query
3. 03-sql-injection-success.png – Successful SQL Injection returning multiple records
Conclusion

A SQL Injection vulnerability was successfully demonstrated in DVWA at the Low security level. A normal user ID returned a single record, while a crafted input altered the SQL query logic and caused multiple records to be returned.

The testing was performed locally in a controlled Kali Linux environment.
