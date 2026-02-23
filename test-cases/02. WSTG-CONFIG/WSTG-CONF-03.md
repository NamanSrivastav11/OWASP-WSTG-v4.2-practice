# WSTG-CONF-03: Test File Extension Handling for Sensitive Information

## Summary

File extensions are critical to how web servers process and serve files, yet they're frequently the source of serious security vulnerabilities. When a web server receives for a file, it uses the file extension to determine which handler (interpreter, plugin, or service) should process it. This seemingly simple mechanism creates a dangerous attack surface when misconfigured or when developers underestimate the security implications.

Attackers exploit file extension handling in multiple ways:

1. **Information Disclosure** - Sensitive files with dangerous extensions (`.inc`, `.bak`, `.env`, `.config`) are left accessible, exposing database credentials, API keys, and internal configuration

2. **Source Code Exposure** - Backup files like `database.php.bak` or `conf.old` are served as plain text instead of being executed, revealing application logic and security flaws

3. **Credential Harvesting** - Include files (`.inc`) containing MySQL connection strings with usernames and passwords are directly readble

4. **Bypass Vulnerabilities** - Double extensions (`.php.jpg`), null bytes, and OS-specific filename handling allow malicious file uploads to execute as code

5. **Business Logic Disclosure** - Configuration files reveal application architecture, database schemas, and technical implementation details


This test case systematically identifies:

- **Forbidden file types** that expose sensitive data ( database includes: configuration files, keys)
- **Backup/old file versions** left behind during deployments
- **Archive files** containing source code or database dumps
- **Double extension bypasses** that defeat upload restrictions
- **Framework-specific vulnerabilities** where certain extensions are unexpectedly executable or readable


## Reference

[OWASP WSTG-CONF-03: Test File Extensions Handling for Sensitive](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information)


## Test Environment

**Target Application:** `demo.testfire.net`

**Authentication:** Not required

**Tools Used:** 

- Web browser
- `curl` for direct file requests
- OWASP ZAP (Fuzzer, Scanner)
- File extension databases (FILExt.com reference)


-------------


## Test Methodology

File extension testing focuses on forced browsing to identify:
1. Sensitive file extensions that shoudld never be accessible
2. Files that contain raw data or credentials
3. Framework/technology bypasses in extension handling
4. OS-specific filename handling issues


-------------


## Validation Steps

### Step 1 - Identify Sensitive File Extensions

**Objective:** Test for file extensions that commonly contain sensitive information.

These extensions typically contain code or configuration that could expose sensitive information:

| Extension | Description | Risk |
|-----------|-------------|------|
| `.asa` | Active Server Pages (ASP source) | Server-side ASP code exposure |
| `.inc` | Include files | Database credentials, config details |
| `.config` | Configuration files | Application settings, connection strings |
| `.php.bak` | Backup of PHP files | Source code exposure |
| `.java.bak` | Backup Java files | Source code exposure |
| `.sql` | SQL files | Database structure, queries |
| `.conf` | Configuration files | Sensitive settings |
| `.env` | Environment variables | Credentials, API keys |
| `.pem`, `.key` | Private key files | Encryption keys, credentials |


**Testing Commands:**

```bash
# Test for include files
curl -i http://testphp.vulnweb.com/config/database.inc
curl -i http://testphp.vulnweb.com/includes/connect.inc
curl -i http://testphp.vulnweb.com/lib/connection.inc
```
```bash
# Test for backup files
curl -i http://testphp.vulnweb.com/index.php.bak
curl -i http://testphp.vulnweb.com/admin.php.old
curl -i http://testphp.vulnweb.com/config.php~
```
```
# Test for config files
curl -i http://testphp.vulnweb.com/web.config
curl -i http://testphp.vulnweb.com/app.config
curl -i http://testphp.vulnweb.com/.env
```
```
# Test for SQL files
curl -i http://testphp.vulnweb.com/database.sql
curl -i http://testphp.vulnweb.com/schema.sql
```
```
# Test for certificate/key files
curl -i http://testphp.vulnweb.com/private.key
curl -i http://testphp.vulnweb.com/certificate.pem
```


<img width="688" height="1094" alt="image" src="https://github.com/user-attachments/assets/4ae40485-1a2e-4453-92a4-3c4a13d8d078" />


<img width="622" height="1087" alt="image" src="https://github.com/user-attachments/assets/bc295163-afc7-401b-af24-ed6717c3c0de" />


<img width="718" height="1084" alt="image" src="https://github.com/user-attachments/assets/29333494-5857-4a50-964f-80eaa0d515ce" />



## Result

| File Tested | HTTP Response | Finding |
|------------|---------------|---------|
| /config/database.inc | 404 Not Found | File not accessible - secure |
| /includes/connect.inc | 404 Not Found | File not accessible - secure |
| /index.php.bak | 404 Not Found | File not accessible - secure |
| /admin.php.old | 404 Not Found | File not accessible - secure |
| /.env | 404 Not Found | File not accessible - secure |
| /database.sql | 404 Not Found | File not accessible - secure |

**Analysis:** The application is properly configured. No sensitive file extensions were accessible. All tested files returned 404 Not Found responses, indicating the server does not expose backup, configuration, or include files.


-------------------------


