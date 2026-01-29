# WSTG-INFO-05 - Review Webpage Content for Information Leakage

## Category

Information Gathering

## Objective

The objective of this test case is to analyze web page content for information disclosure that could aid an attacker in understanding internal application structure, configuration, or sensitive data. Leaked information may be present in:

- HTML comments and metadata
- Inline or extenal JavaScript
- Source map and debug files
- Redirection bodies and response fragments

Reviewing web page content helps identify internal implementation details, sensitive variables, hidden routes, configuration clues, API keys, credentials, or environment-specific data inadvertently exposed to clients.


-------


[OWASP-WSTG-INFO-05: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Webpage_Content_for_Information_Leakage)


-------


## Test Environment

Target Application: `demo.testfire.net`


Authentication: Not required


Tools Used:

- Web browser with developer tools (source view)
- Burp Suite Proxy/Repeater
- Static code review tools
- URL fetch tools (curl, wget) for retrieving resources
- JavaScript analysis utilities

--------


## Background

Developers often include comments, debug code, metadata. or environment information in HTML and JavaScript to assist development or troubleshooting. When this content remains in production, it can provide:

- Internal API endpoints
- Database queries
- Authentication tokens or keys
- Environment configuration
- Hidden application logic or routes
- SOurce map links revealing original source paths

Such leakage expands attacker understanding without exploiting application logic or security controls.


---------

## Test Methodology

1. Retrieve and inspect HTML source of target pages.
2. Identify and review all HTML comments and metadata.
3. Enumerate and fetch external scripts and assets (JS, CSS).
4. Examine JavaSCript code for sensitive data or logic.
5. Identify reference to source map or debug files.
6. Analyze rdirection bodies for data leakage.

Testing emphasizes content analysis using both **manual inspection and automated assistance** where necessary.

--------


## Validation Steps

### Step 1 - Review HTML Source Code

Retrieve the HTML source of application pages using browser `View Source`.

<img width="1555" height="1039" alt="image" src="https://github.com/user-attachments/assets/620e257b-c44f-4e6d-a399-386c10c5bf6c" />


Look for HTML comments with developer notes starting with `<!--`


<img width="1555" height="1039" alt="image" src="https://github.com/user-attachments/assets/48f7a4a8-5c18-4ef2-b920-76bf05c086c4" />


Next, I visited the `/feedback` page, and inspect the page source


<img width="1555" height="1039" alt="image" src="https://github.com/user-attachments/assets/dec60eba-33c2-4079-afca-37fe0a61b2e8" />


Page Source:

<img width="1555" height="1039" alt="image" src="https://github.com/user-attachments/assets/3de8a134-75bd-437c-8711-5efc158018f2" />


Inspecting the page source revealed some developer comments and internal file paths.



------------


## Evidence

The following evidence was collected during testing:

- HTTP source with internal comments


------


## Result

**VULNERABLE**

The application discloses internal information through client-visible webpage content, including comments, JavaScript logic, metadata, and supporting assets.

-----------


## Impact

Information Leakage via webpage content enables attackers to:

- Understand internal aplication logic
- Identify hidden or privileged functionality
- Target backend services directly


-----------


## Mitigation


- Remove all comments and debug code from production assests
- Avoid hard-coding senstitive values in client-side code
- Disable or restrict source map files in production
- Sanitize metadata to remove framework and environment details

Mitigations should be validated by re-reviewing page content post-deployment.


-----------


## Conclusion

The application exposes sensitive internal details through webpage content accessible to all users. This information leakage weakness the securit posture by enabling effective reconnaissance and targeted attacks.
