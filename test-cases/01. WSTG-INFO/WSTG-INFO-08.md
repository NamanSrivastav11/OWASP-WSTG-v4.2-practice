# WSTG-INFO-08 - Fingerprint Web Application Framework

## Category

Information Gathering

## Objective

The objective of this test case is to identify the web application framwork, platform in use. This includes detecting framework-specific headers, default files, directory structures, and responses that reveal the underlying technology.

The goal is to understand the technology stack so later tests can target framework-specific vulnerabilities, misconfigurations, and known weaknesses.


-------------


[OWASP-WSTG-INFO-08: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework)

-------------

## Test Environment

Target Application: `testphp.vulnweb.com`

Authentication: Not required

Tools used:

- Web browser
- OWASP ZAP (Passive Scan, Sites Tree, History)
- Wappalyzer (browser extension)


-----------

## Background

Knowing the application framework helps focus testing on relevant attack surfaces. Frameworks and content management systems often expose:

- Distinctive response headers
- Unique default files or paths
- Signature error messages or directory structures

This test case focuses on identifying these signals without exploiting any vulnerability.

---------------


## Test Methodology

1. Inspect HTTP response headers for framework identifiers
2. Review application responses and default paths for technology clues
3. Use passive fingerprinting tools to corroborate findings
4. Validate results across multiple endpoints


------------


## Validation Steps

### Step 1 - Inspect HTTP Response Headers

Browse the application through OWASP ZAP and capture responses in the **History** tab. Review response headers for framework or platform indicators such as:

- `X-Powered-By`
- `Server`
- Framework-specific cookies


Request:


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/0b2b2742-af29-4dc6-8c85-d3b172a71af9" />


Response:

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/7959ba2c-b4a2-4b91-81cd-dea3ba0ec2d7" />


Upon capturing a response in ZAP History and analyzing the response in the Response Tab, framework-related headers were highlighted.


--------------


### Step 2 - Validting with Passive Fingerprinting

Use `Waplyzer` in the browser to identify detected frameworks, programming languages, and web servers.


<img width="1369" height="927" alt="image" src="https://github.com/user-attachments/assets/710c853c-38b6-4a25-bfc9-ac3f58ae31cf" />


----------


## Evidence

The following evidence was collected during testing:

- Response headers indicating server/framework details
- Wappalyzer detection results validating the framework


-----------


## Result

**NOT VULNERABLE**

The application framwork was successfully fingeprinted. No direct vulnerability was confirmed in this test case, as the focus was on identification rather than exploitation.


--------------


## Impact

Fingerprinting the framework enables testers to:

- Focus on technology-specific attack vectors
- Identify known vulnerabilities for the detected stack
- Prioritize configuration reviews relevant to the framework

Without accurate fingerprinting, tests may miss relevant vulnerabilities or waste effort on irrelevant checks.


-------------


## Mitigation

To reduce unnecessary framework exposure:

- Remove or obfuscate framework-identifying headers
- Disable default pages and simple files
- Ensure custom error pages do not leak stack traces or framework details
- Regurlarly update frameworks and platforms to supported versions

------------

## Conclusion

Framework indicators were identified through headers, default paths, and passive fingerprinting tools, providing a clear understanding of the application's underlying technology stack for subsequent testing.
