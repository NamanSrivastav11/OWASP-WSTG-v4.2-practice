# WSTG-INFO-02 - Fingerprint Web Server

## Category

Information Gathering

## Objective

The objective of this test case is to identify the web server software, version, and related technologies used by the target application. The purpose is to determine whether excessive or unnecessary server identification information is disclosed, which could assist an attacker in tailoring targated attacks.

This test focuses on **passive and low-noise techniques** to fingerprint the server without exploitation.


-------


[OWASP-WSTG-INFO-02: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server)


-------


## Test Environment

Target Application: `testphp.vulnweb.com`


Authentication: Not required


Tools Used:

- Web browse
- Intercepting proxy and repeater (Burp Suite)

--------


## Background

Web servers often disclose identifying information throug HTTP headers, error messages, default pages, or TLS configurations. This information can reveal:


- Web server software (Apache, Nginx, IIS, etc.)
- Version numbers
- Underlying operating systems
- Enables modules or extensions

While server fingeprinting does not constitute a vulnerability, **detailed version disclosure significantly reduces attacker efforts** by enabling rsearch and exploit targeting.


---------

## Test Methodology

The test is performed using passive and low-interaction techniques. The methodology prioritizes observing default server behavior rather than manipulating server state.

--------


## Validation Steps

### Step 1 - Intercept HTTP Response Headers


With Burp Proxy enabled, intercept a standard request to the application:

**Request**
```
GET / HTTP/1.1
Host: testphp.vulnweb.com
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
```

Analyze the HTTP response headers for server identification, including but not limited to:

- `Server`
- `X-Powered-By`
- Framework


Findings:

<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/d12d95a1-6579-4c54-b55e-346b2a78bcdc" />

- Server software is disclosed
- Version numbers are revealed


-----------


2. Analyzing Error Responses

   Request a non-existent resource while interception is enabled

   **Request**
   ```
   GET / HTTES/1.1
   Host: testphp.vulnweb.com
   Cache-Control: max-age=0
   Upgrade-Insecure-Requests: 1
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
   Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
   Accept-Encoding: gzip, deflate, br
   Accept-Language: en-US,en;q=0.9
   Connection: keep-alive

   ```

   Review the response body and headers for:

   - Default error page signatures
   - Platform-specific error formatting
     

   <img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/74c08cc1-c30b-4d9b-a81a-9b76fc0dc57a" />



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
