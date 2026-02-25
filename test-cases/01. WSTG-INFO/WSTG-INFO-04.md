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

### Step 1 - DNS Enumeration and Reverse Lookups

We will try to discover all DNS names associated with the target IP address and identify virtual hosts

Testing Commands:

   ```bash
   # Forward DNS Lookup

   nslookup testphp.vulnweb.com
   ```

   <img width="656" height="232" alt="image" src="https://github.com/user-attachments/assets/07f2c656-3ecb-4b95-beaa-0679b309de01" />


   ```bash
   dig testphp.vulnweb.com
   ```

   <img width="835" height="683" alt="image" src="https://github.com/user-attachments/assets/943c76bd-55c9-4ab5-a863-c075874fcf9f" />



   

   ```bash
   # Reverse DNS lookup

   nslookup 44.228.249.3
   ```
   
   <img width="694" height="129" alt="image" src="https://github.com/user-attachments/assets/b7530caf-20f1-430a-bbd5-dca5fae592e1" />



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
