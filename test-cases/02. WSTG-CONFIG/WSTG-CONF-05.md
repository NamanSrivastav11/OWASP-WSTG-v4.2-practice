# WSTG-CONF-05: Enumerate Infrastructure and Application Admin Interface

## Summary

Administrator interfaces may be present in applications or application servers to allow privileged users to undertake administrative activities. These interfaces often control critical functionality including user account provisioning, site design and layout modifications, data manipulation, and configuration changes.
Testing should be undertaken to identify these admin interfaces and determine if they can be accessed by unauthorized or non-privileged users.

Many organizations fail to adequately protect administrative interfaces through access controls, leading to unauthorized privilege escalation. This test case focuses on discovering hidden administrator interfaces and evaluating their security posture.


## Test Objectives

- Identify hidden or non-abvious administrator interfaces and functionality
- Evaluate access controls protecting administrative functionality
- Determine if administrative interfaces are accessible to unauthorized users


## Reference

[OWASP WSTG-CONF-05: Enumerate Infrastructure and Application Admin Interface](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces)


## Test Environment

**Target Application:** `testphp.vulnweb.com`

**Authentication:** Not required

**Tools Used:**

- Web browser
- curl (HTTP requests)
- OWASP ZAP (spider and scanner)
- DNS/host lookup (`nslookup`, `dig`)


------------

## Test Methodology

1. Enumerate potential admin entry points from application mapping and naming patterns
2. Correlate discovered paths with hostnames, ports, and service behavior
3. Validate access controls and exposure level for each identified interface
4. Classify interfaces by risk (publicly exposed, protected, restricted, or unreachable)


------------

## Validation Steps

### Step 1 - Map Candidate Admin Paths from Existing Surface

Use ZAP Spider and **Sites Tree** to identify paths that suggest administrative functions.
- `/admin`, `/administrator`, `/manage`, `/console`, `/panel`

Found a `/admin` path in the application

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/436cde4e-4f0a-445a-8385-ba0c9751f264" />


-------------


### Step 2 - Directory and File Enumeration

Attempt to guess the paths of administrative interfaces using common naming conventions and framework-specific paths.

**Common Admin paths to test:**

- `/admin`
- `/administrator/`
- `/admin-panel`
- `/adminpanel`
- `/management`
- `/manage`
- `/admin.php`
- `/adminlogin.php`
- `/login.php` (with elevated privileges)
- `/phpmyadmin/` (PHP applications)
- `/phpMyAdmin/`
- `/mysqladmin/`

To enumerate for the above admin paths, we can use a Powershell script:

```powershell
# Test common admin paths
$adminPaths = @(
    "/admin",
    "/admin/",
    "/administrator/",
    "/admin-panel",
    "/adminpanel",
    "/management",
    "/manage",
    "/admin.php",
    "/adminlogin.php",
    "/login.php",
    "/phpmyadmin/",
    "/phpMyAdmin/",
    "/mysqladmin/",
    "/MySQLAdmin/",
    "/dbadmin"
)

$results = @()
foreach ($path in $adminPaths) {
    $url = "http://testphp.vulnweb.com" + $path
    try {
        $response = curl.exe -s -o /dev/null -w "%{http_code}" $url
        $results += @{
            Path = $path
            StatusCode = $response
            Accessible = $response -in @("200", "301", "302")
        }
    } catch {
        $results += @{
            Path = $path
            StatusCode = "Error"
            Accessible = $false
        }
    }
}

# Display results
$results | Format-Table -AutoSize
```

<img width="1530" height="679" alt="image" src="https://github.com/user-attachments/assets/9b6082ec-856b-4997-a7a0-36de6af0ab15" />

<img width="1528" height="674" alt="image" src="https://github.com/user-attachments/assets/e37428a5-d6fe-4c8d-bf4b-7b82be6e7bc7" />

<img width="1529" height="675" alt="image" src="https://github.com/user-attachments/assets/a2887915-de16-4159-acd8-9eb70731eba9" />


| Path | Status Code | Accessible |
|------|-------------|-----------|
| /admin | 301 | Yes |
| /admin/ | 200 | Yes |
| /administrator/ | 404 | No |
| /admin-panel | 404 | No |
| /adminpanel | 404 | No |
| /management | 404 | No |
| /manage | 404 | No |
| /admin.php | 404 | No |
| /adminlogin.php | 404 | No |
| /login.php | 200 | Yes |
| /phpmyadmin/ | 404 | No |
| /phpMyAdmin/ | 404 | No |
| /mysqladmin/ | 404 | No |
| /dbadmin | 404 | No |


------------


### Step 3 - 
