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

Target Application: `testphp.vulnweb.com`

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

### Step 1 - Configure ZAP Automated Scan (Spider)

Launch OWASP ZAP and use **Automated Scan** to target `http://testphp.vulnweb.com`.
Enable the **Spider** during the scan to crawl the application and discover execution paths.

After the spider completes, review the discovered nodes in the **Sites Tree**.

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/d99a20ae-d08e-471e-9c1c-1659a94f282b" />

- Set the URL to attack as:  `http://testphp.vulnweb.com`
- Check the `Use traditional spider` option
- (Optional - `Use the AJAX Spider` to map the `/AJAX` endpoints as well if any found)

Once done, click on **Attack**

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/c541d3d5-c70d-469a-8b05-1404e9506ca1" />


### Step 1 - Review Server Metadata Exposure

Capture responses in OWASP ZAP **History** and inspect for platform-revealing headers such as:

- `Server`
- `X-Powered-By`
- Framework/runtime-specific headers

We can check whether version numbers or technology details 
