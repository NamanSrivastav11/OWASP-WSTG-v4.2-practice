# WSTG-INFO-01 - Conduct Search Engine Discovery Reconnaissance

## Category

Information Gathering

## Objective

Identify sensitive information exposed through search engine indexing that could aid an attacker in understanding the application structure, functionality, or internal implementation.

The goal is to determine wheter publicly indexed contect reveals information that should not be accessible without authentication or authorization.


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



3. Search for sensitive file types:

   ``` site:testphp.vulnweb.com filetype:php```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/9abcaee8-300a-455b-a78e-3a9e47d12dc5" />

   
   ```site:testphp.vulnweb.com filetype:bak```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/26729f56-e48a-4be1-9d55-a71747e663fe" />


   ```site:testphp.vulnweb.com filetype:txt```


   <img width="1610" height="1046" alt="image" src="https://github.com/user-attachments/assets/f91a2cd4-f698-44b1-a190-de6343ba42ac" />


