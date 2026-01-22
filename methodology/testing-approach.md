# Testing Approach - OWASP WSTG v4.2

## Objective

This document defines the testing appreach followed throughout this repository while practicing the OWASP Web Security Testing Guide (WSTG) v4.2
The approach ensures consistency, reproducibility, and evidence-based validation across all documented test cases.


------------


## Testing Philosophy

All testing activities follow these core principles:

- Manual testing is primary and mandatory
- Automation is used only for assistance, not conclusions
- Every test must produce observable evidence


## Scope Definition

Each test case is executed within a clearly defined scope:

- Target application or lab environment
- Specific functionality or endpoint
- Authentication state (unauthenticated / authenicated)
- User role, if applicable

Out-of-scope functionality is explicitly excluded from conclusions.


--------------


## Test Execution Workflow

Each WSTG test case follows the same execution flow:

### 1. Test Case Identification
- Select a single WSTG test case
- Review its objective and testing criteria
- Identify what behavior indicates failure

### 2. Environment Preparation
- Configure intercepting proxy
- Confirm application reachability
- Establish baseline application behavior

### 3. Manual Validation
- Intercept and analyze the HTTP requests and responses
- Manipulate parameters, headers, or workflow as required
- Observe application behavior unser test conditions

### 4. Evidence Collection
- Capture raw HTTP requests and responses
- Take screenshots where visual confirmation is required
- Store payloads and test data used during validation


### 5. Analysis
- Compare observed behavior against expected secure behavior
- Identify deviations or weaknesses
- Eliminate false positives

### 6. Conclusion
- Assign a binary result -> Vulnerable or Not Vulnerable
- Document technical impact and rasoning


---------------


## Evidence Handling

All evidence must be:
- Directly tied to a specific test case
- Clearly labled and timestamped where possible

Evidence without context is treated a invalid.


-------------


## Reporting Standards

Each test case document must include:
- Clear objective
- Step-by-step validation
- Evidence references
- Binary result
- Technical conclusion


## Alignment with OWASP WSTG

This approach is derived directly from the principles outlined in the OWASP Web Security Testing Guide v4.2 and is intended to reflect real-world penetration testing and security assessment practices.
