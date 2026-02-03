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
- Burp Suite (Proxy, Target -> Site Map, Repeater)


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


### Step 1 - Identify Primary Navigation Flow


Browse the application from the landing page and note the main user paths:


- Home page -> Category listing -> Product details
- Search -> Results -> Product details
- Login -> Authenticated pages -> Logout



Capture each request in Burp Proxy and map the sequence in the site map. This establishes the main execution flow of typical user activity.


<img width="1369" height="927" alt="image" src="https://github.com/user-attachments/assets/5634939d-ba32-41ce-bcb5-6112fef6eb9a" />


Visiting the `Categories` tab -> Open the `Paintings` product details.


<img width="1369" height="927" alt="image" src="https://github.com/user-attachments/assets/82ed2c40-ad9a-44c2-8bf2-73178094cf8e" />


<img width="1369" height="927" alt="image" src="https://github.com/user-attachments/assets/c986ac9e-060a-45d7-a58d-bca536b397cc" />


Now, capture this request inside Burp Proxy


<img width="1585" height="980" alt="image" src="https://github.com/user-attachments/assets/8167bbce-c004-484b-8c3b-900fc96546e2" />


<img width="1585" height="980" alt="image" src="https://github.com/user-attachments/assets/35fa5d76-fcf8-4219-8b73-46c719a30440" />


-------------------


### Similarly, browse via the Search box


<img width="1371" height="927" alt="image" src="https://github.com/user-attachments/assets/7c140504-196f-4a98-b1a0-e8c8cae951d0" />

Type in a keyword `hi` and observe the result

<img width="524" height="213" alt="image" src="https://github.com/user-attachments/assets/c146c4f8-59a1-4a3c-8458-eb051a9d7403" />


The application searched for `hi` and displayed a product from category `Paintings`


<img width="1369" height="927" alt="image" src="https://github.com/user-attachments/assets/bf893fdb-6d54-4e86-b9a6-38693e547a20" />


-------------------


-------------


### Step 2 - Trace Multi-step Workflows

Follow a multi-step action such as 
