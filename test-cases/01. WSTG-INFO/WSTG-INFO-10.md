# WSTG-INFO-10 - Map Application Architecture

## Category

Informatinon Gathering

## Objective

The objective of this test case is to map the high-level application architecture by identifying key components, trust boundaries, data flows, and integration points.

The goal is to undersatnd how requests and data move between the client, web server, application logic, data stores, and enternal services so that later testing can focus on critical control points.


--------------


[OWASP-WSTG-INFO-10: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture)


----------------


## Test Environment

Target Application: `testphp.vulnweb.com`

Authentication: Not required

Tools used:

- Web browser
- OWASP ZAP (Spider, Sites Tree, History)
- DNS lookup tools (`nslookup`, `dig`)


-------------


## Background

Understanding architecture helps security testing by revealing where controls should exist and where failures are most likely. Typical architectural emelents include:

- Presentation layer (browser/UI)
- Application layer (routing, business logic, APIs)
- Data layer (database/file storage)
- External services and third-party integrations

This test case focuses on structural mapping rather than exploitation.


------------


## Test Methodology

1. Discover all reachable endpoints and funtional areas
2. Identify likely backend tiers and component responsibilities
3. Trace data flow through user actions and request/response patterns
4. Document trust boundaries and sensitive processing points


-------------


## Enumerate Functional Areas with ZAP Spider

Run OWASP ZAP Spider against the target and review discovered paths in **Sites Tree**. Group discovered endpoints by purpose (content pages, search, authentication, cart, user actions).


<img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/74359bed-578e-459c-9466-49b30d88dc52" />


Grouped functional paths:

<img width="408" height="1083" alt="image" src="https://github.com/user-attachments/assets/e40a301d-6f81-4299-bb98-d133482c7b80" />


--------------


### Evidence

The following evidence was collected during testing:

- ZAP Sites Tree output showing the discovered application surface

-------------

## Impact

Architecture mapping enables testers to:

- Prioritize high-risk components and trust boundaries
- Focus deeper tests on security-critical data flows
- Detect design weaknesses in segmentation or control placement

Without architectural mapping, critical attack paths may be missed.

-------------


## Mitigation

- Enforce security controls consistently across all layers
- Segment internal services and restrict trust between components
- Minimize unnecessary third-party dependencies
- Maintain up-to-date architecture and flow documentation


-----------


## Conclusion

The application's architecture, request flow, and trust boundaries were mapped using observed behavior and passive discovery. This provides a foundation for focused testing of authorization, input handling, and business logic controls.

