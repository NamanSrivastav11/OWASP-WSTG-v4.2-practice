# WSTG-CONF-02 - Test Application Platform Configuration

## Category

Configuration and Deployment Management Testing

## Objective

The objective of this test case is to assess whether the applicatino platform (webserver, runtime, middleware, and deployment settings) is securely configured and does not expose unnecessary information or dangerous defaults.

The goal is to identify platform-level weaknesses that can increase attack surface, disclose sensitive technical details, or weaken application security controls.


------------


[OWASP-WSTG-CONF-02: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration)


------------

## Test Environment

Target Application: `demo.testfire.net`

Authentication: Not required

Tools Used:

- Web browser
- OWASP ZAP (Sites Tree, History)
- HTTP client (`curl` ) for header verification

------------


## Background

Even when application code is secure, weak platform configuration can introduce serious risk.
Common issues include:

- Verbose server banners and version leakage
- Debug or development settings enabled in prosuction
- Insecure HTTP methods left enabled
- Missing or weak security headers
- Default pages or sample resources exposed

This test case focuses on identifying those platform-level weaknesses through safe, observation-based checks.


-----------


## Test Methodology

1. Review platform response behavior and metadata exposure
2. Check for insecure/default HTTP and server configuration indicators
3. Verify platform hardening signals through headers and responses
4. Document configuration weaknesses and security impact


-----------


## Validation Steps


### Step 1 - Review Server Metadata Exposure

Capture responses from `https://demo.testfire.net` in OWASP ZAP **History** and inspect for platform-revealing headers such as:

- `Server`
- `X-Powered-By`
- Framework/runtime-specific headers

We can check whether version numbers or technology details are unnecessarily exposed.

Request:

  <img width="1511" height="1018" alt="image" src="https://github.com/user-attachments/assets/24a76a79-1e9e-4409-8f19-b2d5c3b319c6" />


Response:

  <img width="1512" height="1018" alt="image" src="https://github.com/user-attachments/assets/8c0f55b6-e785-4998-bb27-78ff10c82e81" />


The Captured response header shows the server/platform metadata.


-------------


### Step 2 - Check HTTP Method Configuration

Use `curl` to check whether unnecessary methods are enabled:

- Send an `OPTIONS` request to key endpoints
- Observe allowed methods in `Allow` headers
- Look for if any risky methods (such as `TRACE`, `PUT`, `DELETE`) appear exposed without business need.
  
  > TRACE: Can expose request headers and sensitive data
  > 
  > PUT: Allows uploading/overwriting files
  > 
  > DELETE: Allows removing resources

Command: `curl -i -X OPTIONS https://demo.testfire.net/`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/b19043ac-b5a8-427d-a669-921bafa3a920" />


### Step 3 - Testing multiple endpoints for HTTP methods

We can test different endpoints to identify consistent or inconsistent HTTP method restrictions:

**Commands to run**

```
curl -i -X OPTIONS https://demo.testfire.net/
curl -i -X OPTIONS https://demo.testfire.net/index.jsp
curl -i -X OPTIONS https://demo.testfire.net/account/
curl -i -X OPTIONS https://demo.testfire.net/feedback.jsp
```


Response for `curl -i -X OPTIONS https://demo.testfire.net/index.jsp`

<img width="1166" height="569" alt="image" src="https://github.com/user-attachments/assets/fdf1366d-6666-4331-b2cb-a261394a9331" />

**Findings:**
1. The `Server:` header line (Shows Apache-Coyote)
2. The `HTTP/1.1 200 OK` status
3. `Allow:` header is not shown or missing
4. The Set-Cookie information (shows Secure flags)


Response for `curl -i -X OPTIONS https://demo.testfire.net/account/`

<img width="718" height="171" alt="image" src="https://github.com/user-attachments/assets/6926db1c-dd3e-4e00-b4b9-ecdc49c21f73" />

- Dangerous HTTP Methods Enabled:
    - `PUT` method accessible - allows uploading/modifying account data
    - `DELETE` method accessible - allows deleting account resources
 
<img width="1153" height="628" alt="image" src="https://github.com/user-attachments/assets/8d25e039-a4e8-421d-b645-cb61dc608cdf" />

<img width="1174" height="628" alt="image" src="https://github.com/user-attachments/assets/39646a12-2ce0-4110-a603-7670669f9a1a" />


Similar HTTP methods were enabled for `/feedback.jsp` , `default.jsp` , `robots.txt` , `/swagger/index.html` as well.
