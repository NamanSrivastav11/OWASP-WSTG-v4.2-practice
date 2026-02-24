# WSTG-INFO-04 - Enumerate Applications on Webserver

## Category

Information Gathering

## Objective

The objective of this test case is to identify all applications, directories, virtual hosts, and functional contexts hosted on the same web server as the target application. The goal is to determine whether additional applications or entry points are exposed that expand the overall attack surface.

This test case evaluates whether the server hosts:

- Multiple applications under different paths
- Legacy or test applications
- Administrative or unised functionality
- Misconfigured virtual hosts accessible via the same server


-------

## Reference

[OWASP-WSTG-INFO-04: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/04-Enumerate_Applications_on_Webserver)


-------


## Test Environment

Target Application: `testphp.vulnweb.com`


Authentication: Not required


Tools Used:

- Web browser
- OWASP ZAP (Spider, Sties Tree, History)
- Network scanner (`nmap`)
- `curl` for HTTP testing
- DNS/host discovery tools (`nslookup`, `dig`)

--------


## Background

A single web server may host multiple applications under different paths, hostnames, or ports. Security teams often test only the visible main app and miss:

- Legacy or backup applications
- Admin portals and internal tools
- Alternate virtual hosts on the same IP
- Dev/test/staging interfaces

This test case focuses on mapping those additional applications so later security testing includes all reachable contexts.

Web servers often disclose identifying information throug HTTP headers, error messages, default pages, or TLS configurations. This information can reveal:


- Web server software (Apache, Nginx, IIS, etc.)
- Version numbers
- Underlying operating systems
- Enables modules or extensions

While server fingeprinting does not constitute a vulnerability, **detailed version disclosure significantly reduces attacker efforts** by enabling rsearch and exploit targeting.


---------

## Test Methodology

Web application discovery involves addressing three key scenarios:

1. Non-Standard URLs (Hidden Applications) -> Applications may be hosted at non-obvious URLs (e.g., `/admin`, `/webmail`) and are not referrenced elsewhere.

2. Non-Standard Ports -> Web applications may be hosted on arbitrary TCP ports beyond 80 (HTTP) and 443 (HTTPS), such as 8080, 8000, etc.

3. Virtual Hosts -> A single IP address can host multiple applications via different DNS names using HTTP 1.1 Host headers.

--------


## Validation Steps

### Step 1 - Enumerate Common Application Paths

Here we will try to identify the applications hosted in common directories on the web server.

Testing Commands -> 

1. Test common admin paths
   
   ```bash
   curl -i http://testphp.vulnweb.com/admin
   ```

   <img width="875" height="283" alt="image" src="https://github.com/user-attachments/assets/2e4b04c0-75fe-4b23-8a21-6da4631b1c40" />



   ```bash
   curl -i http://testphp.vulnweb.com/administrator
   ```
   
   <img width="885" height="265" alt="image" src="https://github.com/user-attachments/assets/d54f5939-5b85-4491-8266-ecb47659705f" />



   ```bash
   curl -i http://testphp.vulnweb.com/user
   ```

   <img width="924" height="275" alt="image" src="https://github.com/user-attachments/assets/62ea054e-dd22-4bb0-b16f-2e8df706e2bd" />



   ```bash
   curl -i http://testphp.vulnweb.com/test
   ```

   <img width="870" height="268" alt="image" src="https://github.com/user-attachments/assets/b033fce1-080b-49de-a7b3-6b214c647c81" />



   ```bash
   curl -i http://testphp.vulnweb.com/dev
   ```

   <img width="837" height="270" alt="image" src="https://github.com/user-attachments/assets/c2891657-b108-4120-96b5-17b0587b2623" />



   ```bash
   curl -i http://testphp.vulnweb.com/phpmyadmin
   ```

   <img width="870" height="263" alt="image" src="https://github.com/user-attachments/assets/ca0ab309-60da-4845-be94-6bc21ee878fb" />



2. Test common API paths

   ```bash
   curl -i http://testphp.vulnweb.com/api
   ```

   <img width="942" height="275" alt="image" src="https://github.com/user-attachments/assets/44bca1bb-16b5-454a-bc22-b4177815b5f0" />


   ```bash
   curl -i http://testphp.vulnweb.com/api/v1
   ```

   <img width="879" height="263" alt="image" src="https://github.com/user-attachments/assets/5ce8c2e4-f57d-4f11-adfb-b9e2ffbe1f38" />


**Findings:**

1. `/admin` returned `HTTP/1.1 301 Moved Permanently` which indicates that there is a *301 Redirect* and the application exists.



--------------


### Step 2 - Identify Non-Standard Applications and Virtual Hosts


Discovering applications running on non-obvious paths or alternative virtual hosts.


Testing Commands:


   ```bash
   # Test for redirects (do not follow redirects with -L flag)

   curl -i -L http://testphp.vulnweb.com/
   ```

   <img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/4736f399-0db3-454d-a191-2214fac64604" />


   ```bash
   # Checking for multiple hostnames pointing to the same IP

   nslookup testphp.vulnweb.com
   nslookup www.testphp.vulnweb.com
   ```

   <img width="843" height="205" alt="image" src="https://github.com/user-attachments/assets/a5b2f62c-8430-4f1f-b250-edbd35c825e0" />



   ```bash
   curl -i http://testphp.vulnweb.com
   ```

   <img width="1126" height="419" alt="image" src="https://github.com/user-attachments/assets/25386d0f-3dc0-473d-86f5-e782aaac0ab4" />



-----------


### Step 3 - Directory Brute-Forcing

Systematically discovering hidden directories and applications using OWASP ZAP


1. Open OWASP ZAP


2. Enter the target URL in the "URL to attack" field: `http://testphp.vulnweb.com`

3. Click **Attack** -> **Scan**

4. Navigate to **Tools** -> **Fuzzer**

5. Right-click any request in the History tab

6. Select **Fuzz**

7. Highlight the path portion of the URL

8. Click **Add** to add a payload

9. Select **Fuzzer Wordlist** or upload a custom wordlist with common directories

10. Click **Start Fuzzer**

11. Filter results by HTTP status Code (200, 301, 302, 403)



--------------------------------






-----------------------


### Step 4 - Analyze Application Responses and Fingerprinting




------------


## Evidence

The following evidence was collected during testing:

- HTTP response headers captured via Burp Proxy
- Error responses revealing server characteristics


------


## Result

**VULNERABLE**

The web server discloses identifying information through HTTP response headers and error handling behavior. This confirms that the server can be reliably fingerprinted by an external attacker.


-----------


## Impact

Server fingerprinting enables attackers to:

- Identify server software and technologies in use
- Research known vulnerabilities for specific versions
- Tailor attack payloads and exploitation techniques
- Reduce reconnaissance time and noise


While not directly exploitable on its own, this information disclosure weakens the overall security posture of the application.


-----------


## Mitigation

To reduce server fingerprinting exposure:

- Remove or obfuscate server identification headers such as `Server` and `X-Powered-By`
- Configure custom error pages that do not discloses server or framework details
- Ensure consistent headers behavior across all responses
- Regularly review externally available intelligence sources (e.g., Shodan) for unintended disclosures
- Validate mitigations by re-testing headers and error responses after changes


-----------


## Conclusion

The application's web server exposes identifying information through default HTTP headers and error responses, allowing reliable fingerprinting of the underlying technology stack.
