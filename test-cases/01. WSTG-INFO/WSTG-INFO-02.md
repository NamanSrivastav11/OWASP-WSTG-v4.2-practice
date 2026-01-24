# WSTG-INFO-02 - Fingerprint Web Server

## Category

Information Gathering

## Objective

The objective of this test case is to identify the web server software, version, and related technologies used by the target application. The purpose is to determine whether excessive or unnecessary server identification information is disclosed, which could assist an attacker in tailoring targated attacks.

This test focuses on **passive and low-noise techniques** to fingerprint the server without exploitation.


-------


[OWASP-WSTG-INFO-01: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage)


-------


## Test Environment

Target Application: testphp.vulnweb.com
Authentication: Not required
Tools Used:

- Search engines (Google, Bing)
- Browser

--------


## Test Methodology

Search engine reconnaissance focuses on discovering information that the application owner did not intend to make public but is accessible due to improper indexing or exposure.


The test involves:

- Using targeted search queries [ Google Dorking ]
- Identifying indexed resources, parameters, or sensitive content
- Correlating findings with application behavior


--------


## Validation Steps

1. Perform basic indexed search: Enumerate Intexed Content

   ``` site:testphp.vulnweb.com ```


   <img width="731" height="357" alt="image" src="https://github.com/user-attachments/assets/588734ef-9f36-4f28-8f76-8b8014000fc3" />


   This gives us all the indexed websites that contain the domain name `testphp.vulnweb.com`


    <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/321f0767-e2df-4abf-b696-49a74740e7b8" />


-----------


2. Search for sensitive file types:

   Searching for file extensions commonly associated with backups or development.

   ``` site:testphp.vulnweb.com filetype:php```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/9abcaee8-300a-455b-a78e-3a9e47d12dc5" />

   
   ```site:testphp.vulnweb.com filetype:bak```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/26729f56-e48a-4be1-9d55-a71747e663fe" />


   ```site:testphp.vulnweb.com filetype:txt```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/f91a2cd4-f698-44b1-a190-de6343ba42ac" />



--------------


3. Identify Functional Entry Points

   ``` site:testphp.vulnweb.com inurl:login ```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/f4d26815-0e14-4559-b227-fd9efb6cc615" />


   ``` site:testphp.vulnweb.com inurl:signup ```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/8062d7ff-6698-45b0-a00d-a996ba1a9ffc" />


   ``` site:testphp.vulnweb.com inurl:id= ```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/5cf3e9b6-6271-4361-943c-b11eeeb466e9" />

   

   These queries help identify:

   - Authentication-related endpoints
   - Business logic parameters
   - Direct object references


----------------


### Validating Live Accessibility

On manually visiting selected indexed URLs in the browser confirmed that:

- The resource is accessible without authentication
- The content matches with what was indexed
- The endpoint is functional and not deprecated

   > <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/93897491-bf99-4b94-a888-4df63daabcb2" />



   > <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/869aaddd-f287-4596-9fda-369710bae41a" />



   > <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/6d087d9c-9727-4c5e-b75c-7f46478d10b7" />



------------


## Evidence

The following evidence was collected during testing:

- Indexed URLs revealing internal application endpoints
- Parameterized pages indexed by search engines
- Public access to functional pages without authentication

------


## Result

**VULNERABLE**

Search engine indexing reveals multiple internal and functional endpoints, including parameterized URLs that expose application structure and logic. This information is accessible without authentication and requires no direct interaction with the application.


-----------


## Impact

While search engine discovery does not directly compromise the application, it significantly enhances an attacker's ability to :

- Map application functionality quickly
- Identify attack verctors without active probing
- Discover legacy or hidden endpoints
- Chain with authentication, authorization, or input validation flaws

This exposure lowers the overall security posture of the application.


-----------


## Mitigation

To reduce exposure through search engine indexing:

- Restrict indexing of non-public resources using `robots.txt`
- Apply `noindex`, `nofollow` directives to sensitive pages
- Remove or restrict access to unused legacy, or test endpoints
- Ensure administrative and sensitive functionality requires authentication
- Regurlarly audit indexed content using search engine queries

Mitigation controls should be validated to ensure sensitive resources remain inaccessible even if discovered.


-----------


## Conclusion

The application permits unrestricted search engine indexing of internal and functional endpoints, leading to unnecessary disclosure of application structure and behavior. Proper index controls and access restrictions are required to limit reconnaissance opportunities.
