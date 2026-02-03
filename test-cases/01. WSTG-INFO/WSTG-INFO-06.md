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


### Step 2 - Discovering Application Contexts

Try to navigate to `http://testphp.vulnweb.com/AJAX`

<img width="706" height="715" alt="image" src="https://github.com/user-attachments/assets/170d7412-fd13-46a1-aa20-4be4f7516675" />

`/AJAX` loads successfully.

<img width="1353" height="321" alt="image" src="https://github.com/user-attachments/assets/b54e4475-836d-40cc-89a2-9f1bddf069e4" />


This proves:

- A distinct application context exists
- AJAX-specific endpoints are exposed
- Client-side driven entry points are accessible directly

Now, once `/AJAX` is accessible, browse `/AJAX` with Burp enabled and observe:

- API-style endpoints
- Parameters in POST/GET bodies

Navigate to `artists` -> `lyzae` (open any artist info and intercept the request inside Burp Repeater)

<img width="1235" height="732" alt="image" src="https://github.com/user-attachments/assets/aa3e11db-29e7-4e48-a7a0-d5afb7d39adc" />


<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/47b2b061-0211-48b0-96c3-eea52d61f6c3" />

Similarly, browse the application to look for additional access points:

<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/ce1099d0-30b5-4661-bd96-c547fa9110a6" />


<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/a8bd0a0f-2fef-4b16-b9e1-f0892f09bd9a" />


<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/657fd0f4-9af1-4f95-b33f-f38e1b32bcea" />



------------


## Evidence

The following evidence was collected during testing:

- Burp Proxy traffic captures showing GET/POST requests and parameters
- Application context discovery of `/AJAX` and related API-style endpoints
- Observed requests inputs across query strings, body parameters, and client-driven routes


------


## Result

** NOT VULNERABLE**

This test case focuses on enumerating entry points rather than confirming a security flaw. The application's entry points were successfully identified across multiple contexts and HTTP methods, enabling informed follow-up testing.


-----------


## Impact

Accurately identifying entry points enables testers and attackers to:

- Map the full attack surface for authentication, authorization, and input validation testing
- Identify hidden routes, parameters, and client-side endpoints
- Prioritize deeper testing on high-risk inputs and exposed functionality


While not a vulnerability itself, incomplete entry point discovery can result in missed findings and a false sense of security.


-----------


## Mitigation

To reduce exposure of unnecessary entry points:

- Minimize publicly accessible endpoints and disable unused routes
- Enforce strict input validation and allowlists on all parameters
- Require authentication on sensitive or administrative paths
- Document and monitor all exposed endpoints via API inventories or asset management
- Validate by regularly reviewing traffic logs and automated discovery output


-----------


## Conclusion

Entry points across traditional web pages and AJAX-driven contexts were identified, providing a clear map of inputs for subsequent security teting. No direct vulnerability was confirmed in this.
