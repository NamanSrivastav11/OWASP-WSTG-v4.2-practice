# OWASP Web Security Testing Guide (WSTG) - Overview

## Purpose of the WSTG

The OWASP Web Security Testing Guide (WSTG) is a comprehensive manual for performing structured, manual web application security testing. It defines *what to test*, *why to test*, and *how to validate security controls* across the full lifecycle of a web application.


The gude is designed to standardize testing methodology so that findings are:
- Repeatable
- Verifiable
- Evidence-driven
- Independent of tools and automation bias


The WSTG does not focus on exploitation for impact alone. Its primary goal is **security verification**.


-------


## Scope of the WSTG

The WSTG covers testing across multiple domains of web application, including but not limited to :
- Information exposure
- Configuration and deployment weaknesses
- Authentication and session management
- Input handling and business logic
- Cryptographic implementation
- Client-side security controls


Each area is broken down into **individual test cases**, each representing a single, testable security control or assumption.


--------


## Structure of the WSTG

The guide is organized into catagories, each prefixed with a unique identifier:

- WSTG-INFO - _Information Gathering_
- WSTG-CONFIG - _Configuration and Deployment_
- WSTG-AUTH - _Authentication_
- WSTG-SESS - _Session Management_
- WSTG-INPUT - _Input Validation_
- WSTG-ERROR - _Error Handling_
- WSTG-Crypto - _Cryptography_
- WSTG-BUSLOGIC - _Business Logic_
- WSTG-CLIENT - _Client-Side Testing_

Each test case includes:
- A clear objective
- A defined testing approach
- Expected security behavior
- Indictors of failure


---------


## Methodological Principles

The WSTG enforces th following principles:

1. **Manual First**

   - Manual testing is required to understand application behavior, context, and logic.
   - Automation may assist discovery but cannot replace validation.

2. **Evidence-Based Validation**

   - Every finding must be supported by observable behavior such as HTTP requests, responses, headers, or state changes.

3. **Binary Outcomes**

   - Each test case results in a clear conclusion:
     - Vulnerable
     - Not Vulnerable

4. **Context Awareness**

   - Findings are evaluated within application context, not tool output severity.


------------


## What the WSTG is Not

The WSTG is not:
- A vulnerability scanner
- An exploit development guide
- A checklist without validation
- A severity ranking framework

It is a **testing methodology**, not a vulnerability database.


-----------


The WSTG mirrors professional penetration testing workflows by:
- Enforcing scoped testing
- Requiring proof of validation

Proper applicatinon of the WSTG produces documentation that aligns with internal security assessments, thrid-party audits, and client-facing penetration testing reports.

-----------
