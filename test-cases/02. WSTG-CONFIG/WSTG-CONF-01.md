# WSTG-CONF-01 - Test Network Infrastructure Configuration

## Category

Configuration and Deployment Management Testing

## Objective

The objective of this test case is to review the network and infrastructure configuration that supports the web application to identify exposed services, unnecessary interfaces, and weak defaults.

The goal is to build a baseline of the deployment surface so later tests can focus on misconfigurations that could enable unauthorized access or data exposure.


------------


[OWASP-WSTG-CONF-01](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration)

---------------

## Test Environment

Target Application: `testphp.vulnweb.com`

Authentication: Not required

Tools used:

- Web browser
- OWASP ZAP (Sites Tree, History)
- DNS Lookup tools (`nslookup`, `dig`, `Whois Lookup`)

-----------------


## Background

Infrastructure misconfigurations often expose unnecessary services or management interfaces. Attackers can exploit these to bypass application-level controls or gain access to sensitive systems.

This test case focuses on identifying the visible infrastructure surface in a controlled and authorized manner.

----------------


## Test Methodology

1. Enumerate domains, subdomains, and IPs associated with the target
2. Identify exposed services and ports (only with explicit authorization)
3. Review HTTP response headers and server banners for infrastructure clues
4. Document findings to guide deeper configuration testing


----------------


## Validation Steps

### Step 1 - Identify Target DNS and IP Footprint

Resolve the target domain with `nslookup` or `dig` to identify associated IPs and CNAMEs.
Record any related domains that appear in DNS responses.


Using `NsLookup.io` to find DNS records for the target application

<img width="576" height="305" alt="image" src="https://github.com/user-attachments/assets/1d316647-971c-44f4-90a6-2e562260cf26" />

<img width="1320" height="832" alt="image" src="https://github.com/user-attachments/assets/2dc7b90d-615b-4583-ade0-3aed33ec136a" />

The application's **Address (A) record** was found which displayed the public IP address of the application.

Now, performing a `nslookup` against the found IP address.

<img width="548" height="129" alt="image" src="https://github.com/user-attachments/assets/734777f2-e85d-48f4-9409-ce095df2bc14" />

------------------

Doing a `WHOIS Lookup` against the target application

<img width="1075" height="384" alt="image" src="https://github.com/user-attachments/assets/091f2575-4e06-41b7-a9b5-250b196c7b4b" />

<img width="1903" height="1065" alt="image" src="https://github.com/user-attachments/assets/7a9a4a55-6b57-4fa8-af11-46917699e3d9" />

<img width="1906" height="1065" alt="image" src="https://github.com/user-attachments/assets/15e797c4-202a-497a-8710-df4ca77b1c59" />

WHOIS Lookup showed the `Name Servers`, `IP Address`, `Domain Name`, `Registrar URL`, `Registrar Details`.

-----------------


### Step 2 - Review Server Banner and Response Headers

Use OWASP ZAP **History** to inspect response headers for server banners, reverse proxy indicators, or load balancer hints (e.g., `Server`, `Via`, `X-Forwarded-*`).

Request Header for the URL `http://testphp.vulnweb.com/login.php` - 

<img width="1508" height="948" alt="image" src="https://github.com/user-attachments/assets/f696acae-47ec-4fb1-ab91-dca88a25aa8a" />

Response Header for the URL `http://testphp.vulnweb.com/login.php`

<img width="1508" height="959" alt="image" src="https://github.com/user-attachments/assets/09352a12-c777-43c8-bb14-a7d6a800e920" />


--------------------


### Step 3 - Enumerate Exposed Services (only perform with explicit authorization)

Since we are performing on a vulnerable test web application, we don't require any authorization as such.
So run a limited `nmap` scan against identified IP to detect open ports and services.

Using a `Nmap Online Port Scanner`

<img width="1286" height="646" alt="image" src="https://github.com/user-attachments/assets/5cf03a64-e223-4015-acad-d1e7d70cd318" />


Run the Nmap scan against the found IP address


<img width="1259" height="752" alt="image" src="https://github.com/user-attachments/assets/2a758a4d-02fb-4115-ba15-287da0093075" />

Result showing open ports and sevices.


--------------------


## Evidence

The following evidence was collected during testing:

- DNS resolution output and identified IPs/domiains
- Response headers indicating server architecture
- Service enumeration results


--------------


## Result

**Not Vulnerable**

No unsafe infrastructure configuration was confirmed during this baseline review. The network surface was documented to guide deeper configuration testing.

-------------


## Impact

Wear network/infrastructure configuration can lead to:

- Exposure of administrative interfaces
- Direct access to internal services
- Increased attack surface for exploitation


-----------


## Mitigation

To reduce infrastructure exposure:

- Restrict external access to required services only
- Harden or disable management interfaces
- Enforce network segmentation and firewall rules
- Monitor for unauthorized sevice exposure


------------

## Conclusion

The application's network and infrastructure footprint was mapped using DNS resolution, response header review, WHOIS Lookup, nmap port scan. Findings provide a baseline for deeper configuration and deployment testing.
