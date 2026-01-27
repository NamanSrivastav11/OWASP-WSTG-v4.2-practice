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

No brute forcing or directory enumeration is performed.


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


3. Analyzing **Security TXT** file and **Humans TXT** file

   Inside Burp Repeater, modify the request to  `GET /.well-known/security.txt HTTP/1.1` and send the request

   <img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/7b95b75a-466a-4a3f-ab81-b608baccdf9b" />



   Now, to view the `.humans.txt` file, modify the request as follows:

   `GET / HTTP/1.1` -> `GET /humans.txt HTTP/1.1`

   After that send the request and observe the response


   <img width="1664" height="981" alt="image" src="https://github.com/user-attachments/assets/94500973-0406-47fe-85bc-9f2e2e2a41e2" />


----------------


### Validating Live Accessibility

On manually visiting one the URLs revealed inside the `/.well-known/security.txt` file


<img width="484" height="171" alt="image" src="https://github.com/user-attachments/assets/3fd8b42e-35a6-4388-a46d-a2be7a481ad9" />


- Access the path directly in the browser

  > <img width="1555" height="1039" alt="image" src="https://github.com/user-attachments/assets/73e846de-7389-4ac3-a87d-5de89149c467" />



- The resource is accessible without authentication
- The endpoint is functional



------------


## Evidence

The following evidence was collected during testing:

- HTTP requests and responses for metafiles
- Contents of accessible metafiles
- Direct access attemps to disclosed pathss

------


## Result

**VULNERABLE**

The application exposes webserver metafiles that disclose internal paths and functional endpoints. Several of these paths are accessible without authentication, providing unnecessary insight into application structure.


-----------


## Impact

Information disclosed through metafiles can:

- Reveal hidden or sensitive functionality
- Reduce effort required for attack surface mapping

This exposure lowers the overall security posture of the application.


-----------


## Mitigation


To reduce exposure through search engine indexing:

- Avoid listing sensitive or internal paths in `robots.txt`
- Restrict access to administrative or non-public endpoints regardless of indexing.
- Remove obsolete or unused entries from sitemap files.
- Serve minimal or generic metafile content where possible
- Regurlarly review metafiles as part of deployment and security reviews.


Mitigation must be validated by re-accessing metafiles and confirming the absence of sensitive desclosures.


-----------


## Conclusion

The application exposes webserver metafiles that disclose internal paths and functional details. This information is publicly accessible and can be leveraged during reconnaissance to accelerate targeted attacks.
