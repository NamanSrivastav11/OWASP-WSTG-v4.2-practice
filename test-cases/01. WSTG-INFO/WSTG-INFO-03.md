# WSTG-INFO-03 - Review Webserver Metafiles for Information Leakage

## Category

Information Gathering

## Objective

The objective of this test case is to identify information leakage through webserver metafiles that control crawler behavior or expose internal application structure. Metafiles such as `robots.txt`, `sitemap.xml`, and related resources may unintentionally disclose sensitive paths, hidden functionality, or legacy endpoints.

The goal is to determine whether these files reveal information that assists an attacker in mapping the application or identifying targets for further testing.


-------


[OWASP-WSTG-INFO-03: Reference](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage)


-------


## Test Environment

Target Application: `google.com`


Authentication: Not required


Testing Context: External attacker perspective

Tools Used:

- Web Browser
- Burp Suite (Proxy and Repeater)

--------



## Background

Webserver metafiles are intended to guide search engines and crawlers. However, they are publicly accessible by design. If these files list sensitive directories, hidden endpoints, or administrative paths, they can unintentionally disclose valuable reconnaissance data to attackers.

Unlike search engine discovery, this test involves **directly retrieving known metafile locations** and analyzing their contents for information leakage.


-------------


## Test Methodology

The methodology consists of:
1. Directly requesting common metafiles
2. Analyzing their contents for sensitive disclosures
3. Validating whether disclosed paths are accessible or relevant
4. Correlating findings with actual application behavior

No nrute forcing or directory enumeration is performed.


--------


## Validation Steps

1. Identify Common Metafile Locations

    With Burp Proxy enabled, manually request the following files via the browser:

    `/robots.txt` `/sitemap.xml` `/sitemap_index.xml` `/.well-known/security.txt`


    Open Google webpage, and capture the request inside Burp Suite.

    <img width="769" height="371" alt="image" src="https://github.com/user-attachments/assets/c7e05abb-ecd0-4b81-a51b-5b18d4f0004a" />


    <img width="1670" height="981" alt="image" src="https://github.com/user-attachments/assets/ec699efe-f08a-4bef-bf48-725c4a81dacd" />


    After that, send the request to Burp Repeater


    <img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/fbcde11c-7183-472c-88d4-c86e47fbf067" />


    Change `GET / HTTP/1.1` -> `GET /robots.txt HTTP/1.1` and send the request.

    <img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/1c3e36cb-9b98-499d-b86a-7e447d109385" />


Since `/robots.txt` exists, the findings were:

- `Disallow` entries pointing to:
    - Administrative panels
    - Backup directories
    - Test or development paths
    - Sensitive functionality
- Comments revealing internal context
 
Example indicators of concern:


<img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/20926a4c-d0bf-413d-91f6-23c8d53b6cda" />

-----------


2. Analyzing Sitemap Files

   Sitemaps often reveal the **full intended surface area** of an application, including rarely accessed functionality.


   Modify the same request again with `GET / HTTP/1.1` -> `GET /sitemps.xml HTTP/1.1` and send the request.


   <img width="1920" height="1152" alt="image" src="https://github.com/user-attachments/assets/f02c32ce-c0c3-4ac8-955b-7b64298f8ab4" />

   This revealed some URLS, endpoints that are not easily discoverable through normal navigation, some parameterized URLs or legacy paths.

   

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
