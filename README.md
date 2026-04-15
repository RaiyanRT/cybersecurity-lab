# Cybersecurity Home Lab Portfolio

## About

This repository documents my hands-on cybersecurity work across two different environments:

1. **A 5 virtual machine home lab** built on an older laptop running as a headless server, managed remotely from my main daily driver. The lab mirrors a penetration testing environment with an isolated network, vulnerable targets and defensive monitoring. 
   
2. **Cert IV in Cyber Security Coursework** completed at Victoria University Polytechnic, covering various topics such as penetration testing, network monitoring, incident response, IAM, IT governance and more. 

## Lab Architecture

![Lab Architecture](assets/lab-architecture.png)


## Contents

### Reconnaissance & Information Gathering
- [Information Gathering — Metasploitable 2](reconnaissance/information-gathering-metasploitable.md)
  - Tools: Nmap, WhatWeb, Nikto, DIRB, CeWL

### Web Application Security

**SQL Injection**
- [SQL Injection — DVWA (Low Security)](web-app-security/SQL-injection/Sql-Injections-DVWA.md)
  - OWASP: A03:2021

**Broken Authentication**
- [Broken Auth & Session Hijacking — Mutillidae](web-app-security/broken-authentication/Broken-auth-mutillidae.md)
  - OWASP: A07:2021

**Cross-Site Scripting (XSS)**
- [Cross-Site Scripting — DVWA](web-app-security/XSS/Cross-Site-Scripting-DVWA.md)
  - Reflected, Stored & DOM-based XSS — OWASP: A03:2021

### Incident Response
- [Incident Response Plan](incident-response/incident-response-plan.md)
  - Team structure, communication strategy, NIST framework
- [Malware Simulation — EICAR Test File](incident-response/simulations/malware-simulation.md)
  - Red/Blue team exercise with Wazuh SIEM + Windows Defender
- DoS Attack Simulation (Coming Soon)

**More Write-ups Coming Soon**
