# WSTG-INFO — Information Gathering

## Overview

The Information Gathering phase of the OWASP Web Security Testing Guide (WSTG) focuses on collecting and analyzing publicly accessible information about a target web application before performing active security testing. The objective of this phase is to understand the application’s structure, technologies, entry points, and exposed components without attempting exploitation.

Information gathered during this stage forms the foundation for all subsequent testing activities. Accurate reconnaissance reduces uncertainty, helps define the application attack surface, and enables targeted testing in later phases such as authentication, session management, input validation, and business logic testing.

This directory documents practical execution of the **WSTG-INFO** test cases as defined by the OWASP Web Security Testing Guide v4.2.

---

## Purpose of WSTG-INFO

The primary goals of this phase include:

- Identifying exposed information about the application and infrastructure
- Discovering application entry points and execution paths
- Understanding technologies, frameworks, and architecture in use
- Mapping the overall attack surface prior to vulnerability testing

The outcome of this phase is not exploitation, but **situational awareness**. Each test case contributes to building a complete understanding of how the application operates from an external perspective.

---

## Covered Test Cases

The following WSTG-INFO test cases have been completed and documented in this directory:

- WSTG-INFO-01 — Conduct Search Engine Discovery Reconnaissance
- WSTG-INFO-02 — Fingerprint Web Server
- WSTG-INFO-03 — Review Webserver Metafiles for Information Leakage
- WSTG-INFO-04 — Enumerate Applications on Webserver
- WSTG-INFO-05 — Review Webpage Content for Information Leakage
- WSTG-INFO-06 — Identify Application Entry Points
- WSTG-INFO-07 — Map Execution Paths Through Application
- WSTG-INFO-08 — Fingerprint Web Application Framework

Each test case includes:
- Objective and background
- Testing methodology
- Validation steps
- Evidence and observations
- Result and impact analysis
- Mitigation considerations

---

## Testing Summary

The checklist below represents the completed Information Gathering phase, summarizing all test cases performed during this stage.

WSTG-INFO Checklist

<img width="1092" height="739" alt="Screenshot 2026-02-06 at 1 19 43 AM" src="https://github.com/user-attachments/assets/0696265f-9210-4fea-944d-5efec28623f7" />

---
