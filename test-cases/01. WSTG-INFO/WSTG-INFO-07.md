# WSTG-INFO-07 - Map Execution Paths through Application

## Category

Information Gathering

## Objective

The objective of this test case is to map how the application executes across different pages, features, and workflows. This includes understanding how requests move between endpoints, which parameters influence behavior and where  transitions(redirects, forwards, and workflow steps) occur.

The goal is to build a clear execution flow so later tests can target authorization checks, input validation, and business logic decisions that depend on the path a user takes.


--------------


[OWASP-WSTG-INFO-07: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application)


------------------


## Test Environment

Target Application: `testphp.vulnweb.com`

Authentication: Not required

Tools used:

- Web browser
- OWASP ZAP (Automated Scan with Spider, Site Tree, History, Manual Request Editor)


-------------


## Background

Many vulnerabilities appear only when a user follows a specific sequence of actions.
Mapping executino paths reveals:

- Multi-step workflows that change state across requests
- Where parameters control access or branching logic
- Redirects and transitioins that may bypass controls

This test case focuses on understanding the paths users can take, not on exploiting a specific flaw.


-------------


## Test Methodology

1. Identify the application's main navigation paths
2. Trace multi-step workflows and state transitions
3. Record redirects, forwards, and parameters that influence routing
4. Map alternate or hidden paths discovered via site mapping


-------------


## Validation Steps


### Step 1 - Configure ZAP Automated Scan (Spider)


Launch OWASP ZAP and use **Automated Scan** to target `http://testphp.vulnweb.com`. 
Enable the **Spider** during the scan to crawl the application and discover execution paths.

<img width="1626" height="478" alt="image" src="https://github.com/user-attachments/assets/63c79ae2-b02f-4045-9219-e29f93cfedbb" />


URL to attack - `http.testphp.vulnweb.com` -> Start the attack.

Alternatively, 
Go to **Tools** -> Select **Spider**


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/75e30208-2950-4618-8a95-fafd381560f0" />


The Spider menu will appear

<img width="863" height="554" alt="image" src="https://github.com/user-attachments/assets/b8b94e81-b7c6-40a8-b9cb-e27bb7ca6b12" />


Type in the target site - `http.testphp.vulnweb.com` and Start the scan

<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/3ca03777-0277-4ec9-8a00-bb2dad93daca" />

Once the scan is completed, this establishes the baseline execution flow discovered through automated crawling.


-------------------


### Step 2 - Validate Primary Navigation Flow in Sites Tree


Use the **Sites Tree** and **History** tabs to trace to primary navigation paths captured by the spider:

- Home page -> Category listing -> Product details
- Search -> Results -> Product details
- Login -> Authenticated pages -> Logout


<img width="547" height="973" alt="image" src="https://github.com/user-attachments/assets/fe150d71-a580-490e-8c3c-7aa82a467b77" />


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/95e3ddde-4301-4320-b6a9-68a8e14b5fe0" />



-------------


## EVIDENCE

The following evidence was collected during testing:

- ZAP Automated Scan results with Spider-discovered paths in the Sites Tree
- Captured request sequences for search and product navigation
- Spider was able to map various execution paths throughout the application


------------


## Result

**Not Vulnerable**

The execution paths were successfully mapped and documented. No direct vulnerability was confirmed in this test case, as the goal was to understand application flow rather than exploit it.


-------------


## Impact

Mapping execution paths enables testers to:

- Target workflow-dependent logic and authorization controls
- Identify where users might skip steps or access restricted areas
- Detect inconsistent behavior based on request order

Without this map, critical business logic flaws can be overlooked.


--------------


## Mitigation

To reduce risk from complex execution paths:

- Enforce server-side workflow validation for multi-step actions
- Ensure sensitive functionality cannot be accessed out of sequence
- Apply consistent authorization checks across all routes
- Log and monitor unexpected request sequences


---------------


## Conclusion

The application's main execution paths were mapped across navigation, search, and user actions. This provides a baseline for deeper security testing focused on workflow integrity and authorization controls.
