# Penetration Testing Report

## Footprinting & Network Scanning Phases

**W2-PM-FINAL | Cybersecurity | Networkwalks**

| Field                  | Details                                                   |
| ---------------------- | --------------------------------------------------------- |
| **Pentester Name**     | Lucky Samuel                                              |
| **Program/Batch**      | B083-Networkwalks                                         |
| **Date**               | 18 September 2026                                         |
| **Modules**            | W2-PM1 (Multiple Kali Tools) and W2-PM5 (Zenmap Scanning) |
| **Client/Target**      | Networkwalks (written permission) and own local LAN       |
| **Permission Secured** | Yes                                                       |

### Phases Covered

* **Phase 1:** Reconnaissance & Footprinting
* **Phase 2:** Scanning & Network Discovery
* **Phase 3:** Vulnerability Assessment — In Progress
* **Phase 4:** Exploitation — In Progress
* **Phase 5:** Reporting — In Progress

---

## 1. Liability Disclaimer

All activities documented in this report were performed for authorized cybersecurity training and assessment purposes.

Testing of Networkwalks was conducted with written permission, while network scanning was performed against the authorized local network environment.

No unauthorized access, exploitation, service disruption, or destructive activity was performed.

The information presented in this report represents observations obtained during the assessment and should not be interpreted as confirmation of exploitable vulnerabilities unless explicitly stated.

---

## 2. Introduction

This report documents the practical activities completed during the **Footprinting, Reconnaissance, and Network Scanning** phases of the Networkwalks cybersecurity training program.

The main objective was to gather publicly available information about the authorized target, identify technologies and network-related information, and perform basic network discovery using Kali Linux and Zenmap.

The reconnaissance phase involved the use of several tools:

* WHOIS
* WhatWeb
* Nslookup
* cURL
* Wafw00f
* DNSRecon

Zenmap was then used to perform network scanning on the authorized local network.

The results provide an overview of the information that can be identified before moving into later penetration testing phases.

---

## 3. Tools Used

| Tool         | Purpose                                                         |
| ------------ | --------------------------------------------------------------- |
| **WHOIS**    | Obtaining domain registration and ownership-related information |
| **WhatWeb**  | Identifying website technologies and server information         |
| **Nslookup** | Resolving domain names to IP addresses                          |
| **cURL**     | Inspecting HTTP response headers and server responses           |
| **Wafw00f**  | Detecting Web Application Firewall technologies                 |
| **DNSRecon** | Enumerating DNS records and infrastructure information          |
| **Zenmap**   | Network scanning and host discovery                             |

---

# 4. Activities Performed

## 4.1 Footprinting & Reconnaissance

The following tools were used to gather information about the authorized Networkwalks website.

---

## 4.1.1 WHOIS

### Command Used

```bash
whois networkwalks.com
```

### Information Obtained

* **Domain:** NETWORKWALKS.COM
* **Registrar:** GoDaddy.com, LLC
* **Domain Creation Date:** 6 November 2019
* **Domain Expiry Date:** 6 November 2027
* **Last Updated:** 12 November 2025
* **WHOIS Server:** whois.godaddy.com
* **Domain Status:** clientDeleteProhibited, clientRenewProhibited, clientTransferProhibited, clientUpdateProhibited
* **Nameservers:**

  * NS6135.HOSTGATOR.COM
  * NS6136.HOSTGATOR.COM
* **DNSSEC:** Unsigned
* **Registrant Information:** Registration Private / Domains By Proxy, LLC

### Observation

The WHOIS lookup showed that domain registration details were protected through a privacy service.

The information also identified the domain registrar and nameserver infrastructure.

---

## 4.1.2 WhatWeb

### Command Used

```bash
whatweb https://networkwalks.com
```

### Technologies and Information Identified

* **Web Server:** Apache
* **CMS:** WordPress 7.1.1
* **WordPress Download Manager:** 3.3.58
* **jQuery:** 3.7.1
* **Bootstrap:** 7.1.1
* **Google Tag Manager**
* **HTML5**
* **Open Graph Protocol**
* **HTTPS**
* **Website Title:** Networkwalks Academy
* **IP Address:** 192.232.216.135
* **Cookie:** `__wpdm_client`
* **Email:** [info@networkwalks.com](mailto:info@networkwalks.com)

The HTTP version of the website redirected to HTTPS with a **301 response**, while the HTTPS version returned a **200 OK response**.

### Observation

The results provide information about the technologies used by the website.

Technology identification can help a security tester understand the potential attack surface. However, identifying a technology or version alone does **not** confirm that the software is vulnerable.

---

## 4.1.3 Nslookup

### Command Used

```bash
nslookup networkwalks.com 8.8.8.8
```

### Result

| Item        | Result          |
| ----------- | --------------- |
| DNS Server  | 8.8.8.8         |
| Resolved IP | 192.232.216.135 |

### Observation

The lookup confirmed the public IP address associated with the domain.

---

## 4.1.4 cURL

### Command Used

```bash
curl -I https://networkwalks.com
```

### HTTP Response

The HTTP response returned:

```text
HTTP/2 200 OK
```

### Important Information Observed

* **Server:** Apache
* **Content-Type:** `text/html; charset=UTF-8`
* **Referrer-Policy:** `no-referrer-when-downgrade`
* **Permissions-Policy**
* **X-Endurance-Cache-Level:** `0`
* **X-Nginx-Cache:** `WordPress`
* **WordPress REST API:** `/wp-json/`
* **WordPress Page API:** `/wp-json/wp/v2/pages/53`
* **Cookie:** `__wpdm_client`
* **Cookie Attributes:** Secure and HttpOnly

### Observation

The cURL request showed that server and application-related information was included in the HTTP response.

It also revealed references to the WordPress REST API.

This information may be useful during further authorized enumeration.

---

## 4.1.5 Wafw00f

### Command Used

```bash
wafw00f https://networkwalks.com
```

### Result

```text
The site https://networkwalks.com is behind ModSecurity (SpiderLabs) WAF.
Number of requests: 2
```

### Observation

A **ModSecurity (SpiderLabs) Web Application Firewall (WAF)** was detected in front of the website.

The presence of a WAF provides an additional security layer for filtering and monitoring web traffic.

Detection of a WAF does not independently indicate that a website is either vulnerable or secure.

---

## 4.1.6 DNSRecon

### Command Used

```bash
dnsrecon -d networkwalks.com
```

### DNS Information Identified

| Record/Information       | Result                             |
| ------------------------ | ---------------------------------- |
| **SOA**                  | ns6135.hostgator.com               |
| **SOA IP**               | 50.87.144.87                       |
| **NS**                   | ns6136.hostgator.com               |
| **NS IP**                | 192.168.216.131                    |
| **NS**                   | ns6135.hostgator.com               |
| **NS IP**                | 50.87.144.87                       |
| **DNS Software/Version** | BIND 9.16.23-RH                    |
| **MX**                   | mail.networkwalks.com              |
| **MX IP**                | 192.232.216.135                    |
| **A Record**             | networkwalks.com → 192.232.216.135 |
| **TXT**                  | Google site verification           |
| **TXT**                  | SPF policy                         |
| **SRV Records**          | Autodiscover-related services      |
| **DNSSEC**               | No answer returned                 |

### Observation

DNS reconnaissance revealed several records associated with the domain, including:

* Authoritative nameservers
* Mail infrastructure
* TXT records
* SPF information
* SRV records
* A records

The BIND version was also disclosed during enumeration.

Such information can be useful during further authorized security assessment because it provides additional information about the underlying infrastructure.

---

# 4.2 Network Scanning With Zenmap

The second phase involved network discovery using **Zenmap**.

The scan was performed against the authorized local network environment.

### Nmap/Zenmap Result

```text
Starting Nmap 7.991 ( https://nmap.org ) at 2026-09-18 20:07 +0100

Nmap scan report for 192.168.56.1

Host is up.

Nmap done: 1 IP address (1 host up) scanned in 1.31 seconds
```

### Findings

| Finding                   | Result       |
| ------------------------- | ------------ |
| **Target Scanned**        | 192.168.56.1 |
| **Host Status**           | Up           |
| **IP Addresses Scanned**  | 1            |
| **Live Hosts Identified** | 1            |
| **Scan Duration**         | 1.31 seconds |

### Observation

The scan successfully confirmed that the host at **192.168.56.1** was active.

The result represents basic host discovery only. No exploitation or vulnerability testing was performed during this scan.

Because only one IP address was scanned, the result does not provide a complete picture of the entire local network.

A wider authorized network range would be required to identify additional devices.

---

# 5. Risk Analysis

The following observations were identified during the reconnaissance and network scanning activities.

| No. | Observation                             | Evidence                                                         | Potential Security Impact                                                                   | Risk   |
| --: | --------------------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------ |
|   1 | Web technology information exposed      | WordPress 7.1.1 and WordPress Download Manager 3.3.58 identified | May help attackers identify software-specific attack surfaces                               | Medium |
|   2 | Server IP identifiable                  | 192.232.216.135 identified                                       | May assist further infrastructure reconnaissance                                            | Low    |
|   3 | HTTP technical information exposed      | Apache server and WordPress-related response headers identified  | May provide useful information for further enumeration                                      | Low    |
|   4 | WAF technology identifiable             | ModSecurity (SpiderLabs) detected                                | WAF technology and configuration characteristics may be investigated during further testing | Low    |
|   5 | DNS infrastructure exposed              | DNS, mail, TXT, SPF, and SRV records identified                  | DNS information can reveal details about organizational infrastructure                      | Medium |
|   6 | DNS software version disclosed          | BIND 9.16.23-RH identified                                       | Version information can assist targeted vulnerability research if applicable                | Low    |
|   7 | Active host identified on local network | 192.168.56.1 identified as an active host                        | Network discovery can help identify devices that should be known and authorized             | Low    |

> **Note:** Risk ratings represent the potential security relevance of the observations and do not confirm that the identified systems or services are exploitable.

No critical vulnerability was confirmed during the reconnaissance and scanning phases.

---

# 6. Recommendations

## 6.1 Review Publicly Exposed Technology Information

Organizations should review the amount of technology information exposed through web pages, HTTP response headers, DNS records, and other publicly accessible services.

## 6.2 Keep Software Updated

WordPress, plugins, web servers, DNS software, and other infrastructure components should be regularly updated and monitored for security advisories.

## 6.3 Review HTTP Security Headers

HTTP response headers should be reviewed to determine whether unnecessary information is being exposed and whether appropriate security-related headers are configured.

## 6.4 Review DNS Records

Public DNS records should be reviewed regularly to ensure that only necessary records are exposed and that outdated or unnecessary entries are removed.

## 6.5 Properly Configure and Monitor the WAF

The detected ModSecurity WAF should be properly configured, monitored, and regularly reviewed to ensure that it continues to provide effective protection against malicious web traffic.

## 6.6 Perform Regular Internal Network Discovery

Authorized network scans should be performed periodically to identify active hosts and maintain an accurate understanding of the internal network environment.

## 6.7 Investigate Unknown Devices

Any active device discovered on an internal network that is not properly documented or authorized should be investigated by the appropriate network administrator.

## 6.8 Maintain Accurate Network Documentation

Network administrators should maintain updated documentation of authorized hosts, IP addresses, network ranges, servers, and other infrastructure.

## 6.9 Conduct Further Security Testing With Authorization

The findings from reconnaissance and scanning can be used to guide later vulnerability assessment activities.

Any additional testing should remain within the approved scope and authorization.

---

# 7. Conclusion

The Week 2 practical activities successfully covered the **Footprinting, Reconnaissance, and Network Scanning** phases of the penetration testing process.

Six Kali Linux reconnaissance tools were used to gather information about the authorized Networkwalks website.

The assessment identified:

* Domain registration information
* Web technologies
* Public IP address
* HTTP response information
* WAF technology
* DNS infrastructure

Zenmap was also used to perform network discovery on the authorized local network. The scan successfully identified **192.168.56.1** as an active host.

The activities demonstrated how information can be collected before moving into vulnerability assessment and exploitation phases.

The results also showed the importance of:

* Accurate documentation
* Proper authorization
* Controlled security testing
* Network visibility
* Avoiding assumptions about vulnerabilities without appropriate validation

Further penetration testing phases can build on these findings to assess the security of the identified services within the approved scope.

---

## 8. Evidence

The following evidence can be included in this repository:
<img width="1366" height="662" alt="whois" src="https://github.com/user-attachments/assets/aab1affb-627e-4052-b71e-252358879f13" />


<img width="1356" height="744" alt="nmap" src="https://github.com/user-attachments/assets/64dbc11e-9022-4f1c-833d-fd987dc81d2f" />
<img width="360" height="492" alt="topo" src="https://github.com/user-attachments/assets/af1c7399-48fb-4866-8b2a-008fcd745683" />
<img width="1366" height="662" alt="whatweb" src="https://github.com/user-attachments/assets/3d5926ac-146a-45f8-9757-b7622d4d73a7" />
<img width="1366" height="662" alt="ns" src="https://github.com/user-attachments/assets/7ed0ebbd-ec8b-46ee-8fc1-9e27bbffbefa" />
<img width="1366" height="662" alt="curl" src="https://github.com/user-attachments/assets/84ad42f7-b847-4370-9fd5-9fd7e51ba586" />
<img width="1366" height="662" alt="dns" src="https://github.com/user-attachments/assets/377b0b2a-abb6-42fd-ae52-766aba660b0f" />
<img width="1366" height="662" alt="waf" src="https://github.com/user-attachments/assets/9996334c-f80a-4fc8-adea-0d28b5421887" />
<img width="1366" height="662" alt="dns0" src="https://github.com/user-attachments/assets/7107d729-cdf6-41f7-8b6a-041a01ca50e0" />
<img width="1366" height="627" alt="zenmap" src="https://github.com/user-attachments/assets/6bed3a9e-8c22-40cc-9e5e-381bec902cc1" />
<img width="1366" height="627" alt="zenm" src="https://github.com/user-attachments/assets/c089ebe7-79d4-44d0-a775-b37bc877b7fd" />

---

## 9. Skills Demonstrated

Through this practical assessment, the following cybersecurity skills were demonstrated:

* Passive reconnaissance
* Active reconnaissance
* Domain enumeration
* DNS enumeration
* Website technology identification
* HTTP header analysis
* WAF detection
* Network host discovery
* Nmap/Zenmap usage
* Security observation and risk analysis
* Penetration testing documentation
* Responsible and authorized security testing

---

## 10. Environment

**Operating System:** Kali Linux

**Scanning Tool:** Zenmap / Nmap 7.991

**Target:** Authorized Networkwalks website

**Network Target:** Authorized local network

**Assessment Date:** 18 September 2026

**Authorization:** Written permission secured




## 👤 Author

**Samuel Lucky**
Cybersecurity Professional — **B083**

🔗 **LinkedIn:** https://www.linkedin.com/in/lucky-samuel-4bb397296

---

## 📌 Project Information

| Field          | Details                       |
| -------------- | ----------------------------- |
| **Program**    | Cybersecurity at Networkwalks |
| **Week**       | 02                            |
| **Project**    | Penetration Testing Report    |
| **Repository** | GitHub                        |

---
