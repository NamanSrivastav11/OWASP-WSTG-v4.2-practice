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

Command: `curl -i -X OPTIONS https://demo.testfire.net/`

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/b19043ac-b5a8-427d-a669-921bafa3a920" />
