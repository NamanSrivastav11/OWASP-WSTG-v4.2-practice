# WSTG-INFO-06 - Identify Application Entry Points

## Category

Information Gathering

## Objective

The objective of this test case is to identify all possible entry points into the web application. Entry points include URLs, parameters, HTTP methods, headers, cookies, and client-side interfaces through which user-supplied data enters the application and influences server-side processing.

The goal is to build a complete map of interaction surfaces that can later be assessed for authentication, authorization, input validation, and business logic flaws.


-------


[OWASP-WSTG-INFO-06: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points)

-------


## Test Environment

Target Application: `testphp.vulnweb.com`


Authentication: Not required


Tools Used:

- Web browser
- Burp Suite (Proxy, Repeater, Target -> Site map)

--------


## Background

Every vulnerability requires and entry point. Missing even a single parameter, endpoint, or HTTP method can lead to incomplete testing and false confidence.

Modern applications expos entry points through:

- Query parameters and form fields
- Cookies and headers
- Client-side routing and JavaScript-driven requests

This test case focuses on identifying possible entry and injection points through request and response analysis.


---------

## Test Methodology


1. Observing normal application usage
2. Capturing all requests and parameters
3. Enumerating distinct endpoints and inputs
4. Validating where and how user input is accepted


--------


## Validation Steps

### Step 1 - Capture Baseline Traffic


Browser the application normally with Burp Proxy enabled, visiting all accessible pages and features.

<img width="1920" height="1153" alt="image" src="https://github.com/user-attachments/assets/8225bdc7-c18e-4130-b67a-96d6b9c11028" />

Look for distinct URLs and endpoints; `GET` and `POST` requests; form submissions and redirects

`GET` request:


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/b09e514a-a65a-4d5e-a729-d7a4e6296647" />



`POST` request:


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/b939c6b8-274e-4459-a9ce-6b64760e0918" />


The `POST` request uses `query` parameters and `body` parameters in the request.

<img width="338" height="503" alt="image" src="https://github.com/user-attachments/assets/a8ced3b9-6d6f-4036-b813-a889bad7cea5" />


Each unique URL and request represents a potential application entry point.


-----------


### Step 2 - Enumerate Parameters

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
