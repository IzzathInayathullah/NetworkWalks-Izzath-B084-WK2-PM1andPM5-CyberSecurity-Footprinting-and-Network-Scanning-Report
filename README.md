# 🔍 Cybersecurity Footprinting and Network Scanning Report

Conducting cybersecurity footprinting/reconnaissance and network scanning in a controlled environment to identify target information, discover active hosts, analyze network services and writing a detailed report.

<div align="center">

# PENETRATION TESTING REPORT

### W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS

| Pentester Name (Cybersecurity Professional) | Izzath Inayathullah                                   |
|---------------------------------------------|-------------------------------------------------------|
| Program/Batch                               | B084-Networkwalks                                     |
| Date                                        | 10 November 2026                                      |
| Modules completed                           | W2-PM1 (Multiple Kali Tools)<br>W2-PM5 (Zenmap Scanning) |
| Client/Target                               | 1. Networkwalks (secured written permission already)<br>2. Virtual LAN Network |
| Permission secured from client?             | Yes                                                   |
| Phases covered                              | Phase 1: Reconnaissance & Footprinting<br>Phase 2: Scanning & Network Discovery<br>Phase 3-5: In Progress |

</div>
<br>

## 1. Liability Disclaimer

I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The repo publisher(myself), instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.


## 2. Introduction

This report covers footprinting/reconnaissance the networkwalks.com domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.


## 3. Tools Used

The table below lists each tool used in this report and its purpose.

| Tool                 | Purpose                                                          |
|----------------------|------------------------------------------------------------------|
| Kali Linux & Windows | Operating systems used for reconnaissance activities             |
| WHOIS                | Find domain registration details (owner, dates, name servers).   |
| whatweb              | Fingerprint web technologies (server, CMS, plugins, IP).         |
| nslookup             | Resolve the domain name to its IP address using DNS.             |
|curl -I               | Read the HTTP response headers of the website.                   |
|wafw00f               | Detect whether a Web Application Firewall protects the site.     |
|dnsrecon              | Enumerate all DNS records (NS, MX, SPF, TXT, SRV).               |
|Zenmap (Nmap GUI)     | Scan the local subnet to find live hosts, IPs and MAC addresses. |
| Windows CMD          | Local IP and MAC address identification                          |
<br>

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed reconnaissance against the `networkwalks.com` domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f and DNSRecon**. Each tool was used to collect a different type of information about the target.

First, I used **WHOIS** to obtain publicly available domain registration information and identify the domain’s name servers. The results provided information about the domain registration and hosting infrastructure.

I then used **WhatWeb** to identify technologies used by the website. The results identified **WordPress 7.1.4** and **WP Download Manager 3.3.76**, along with other information exposed by the website.

Using **Nslookup**, I resolved the domain name to its IP address. The provided result identified **192.232.216.135**.

I used **Curl** with the `-I` option to inspect the HTTP response headers. This provided additional information about the web application and exposed the WordPress REST API endpoint `/wp-json/`.

Next, I used **Wafw00f** to determine whether a Web Application Firewall was protecting the website. The result identified **ModSecurity (SpiderLabs)**.

Finally, I used **DNSRecon** to enumerate DNS records. The results provided information relating to name servers, mail servers, SPF/TXT records, service records and DNS software information.


#### 4.2 Commands Used

The following commands were used in Kali Linux to perform the footprinting and reconnaissance activities:

**1. WHOIS – Domain Registration Information**

```bash
whois networkwalks.com
```

**2. WhatWeb – Website Technology Identification**

```bash
whatweb https://networkwalks.com
```

**3. Nslookup – Domain Name Resolution**

```bash
nslookup networkwalks.com
```

**4. cURL – HTTP Response Header Inspection**

```bash
curl -I https://networkwalks.com
```

**5. WAFW00F – Web Application Firewall Detection**

```bash
wafw00f https://networkwalks.com
```

**6. DNSRecon – DNS Record Enumeration**

```bash
dnsrecon -d networkwalks.com
```

These commands were used to collect publicly available information about the target domain, identify web technologies, inspect HTTP responses, detect WAF technology and examine DNS records.


### 4.3 Network Scanning with Zenmap

For the second activity, I used Zenmap to perform network discovery on the Virtual network. The practical required me to identify the Virtual PC IP address and subnet, discover live hosts, identify their IP and MAC addresses, and generate a network topology.

I used the following commands during this activity:

**1. Identify the Windows network configuration**

```cmd
ipconfig
```

**2. Zenmap – Network Host Discovery**

The following Nmap command corresponds to the Ping Scan option in Zenmap:

```bash
nmap -sn 10.0.0.0/24
```
The example results provided in the practical identified three live hosts:

- `10.0.0.1`
- `10.0.0.10`
- `10.0.0.2`

The example results also included three MAC addresses.

The `-sn` option performs host discovery without conducting a port scan. The subnet `10.0.0.0/24` should be replaced with the actual subnet identified during the practical, if different.

**3. Network Topology Visualization**

After completing the host discovery scan, I used Zenmap's **Topology** tab to visualize the discovered network devices, enabled the legend and saved the resulting topology as a PNG image.

 
## 5. Risk Analysis / Impact

Based on the information collected during the footprinting and network scanning activities, I identified the following potential risks.

| **\#** | **Risk / Finding**                           | **Evidence / Observation**                                  | **Potential Impact**                                                                                            | **Risk Level** |
|--------|----------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|----------------|
| 1      | Web technology information exposed           | WhatWeb identified WordPress and WP Download Manager        | Attackers may use exposed technology/version information to identify software requiring further security review | **Medium**   |
| 2      | Server IP address identifiable               | Nslookup resolved the domain to `192.232.216.135`           | Provides information about the network location of the web service                                              | **Low**      |
| 3      | HTTP technical information exposed           | Curl returned HTTP response headers and exposed `/wp-json/` | May assist technology fingerprinting and further enumeration                                                    | **Low**      |
| 4      | WAF technology identifiable                  | Wafw00f identified ModSecurity (SpiderLabs)                 | Reveals information about the web application’s security architecture                                           | **Low**      |
| 5      | DNS infrastructure information exposed       | DNSRecon identified DNS, mail and service-related records   | DNS information can help build a broader infrastructure profile                                                 | **Medium**   |
| 6      | Multiple live hosts visible on local network | Zenmap identified three live hosts in the example network    | Unknown or unauthorized devices may potentially be present on a network                                         | **Medium**   |

**Risk level key:** ● Critical ● Medium ● Low
<br>
The risks above are observations from the footprinting and scanning exercises, not confirmed vulnerabilities.

The practical exercises primarily involved information gathering and host discovery. No exploitation or vulnerability validation was performed as part of these two modules.

Therefore, the presence of information such as a software version, IP address or DNS record does not by itself mean that the system is vulnerable. Further authorized security testing would be required to confirm any actual vulnerability.


## 6. Recommendations

Based on the observations from these activities, I recommend the following security improvements:

1.  **Review publicly exposed technology information**  
    Organizations should regularly review what information about their web technologies, CMS and plugins is publicly visible.

2.  **Keep software updated**  
    CMS platforms, plugins and other web technologies should be regularly updated and reviewed against current security advisories.

3.  **Review HTTP headers**  
    HTTP response headers should be reviewed to determine whether unnecessary technical information is being exposed.

4.  **Review DNS records regularly**  
    DNS records should be checked periodically to ensure that only required information and services are publicly exposed.

5.  **Properly configure and monitor the WAF**  
    Keep the WAF (ModSecurity) enabled and tuned, since it already blocks naive attacks.

6.  **Perform regular internal network discovery**  
    Organizations should periodically scan their own networks to identify active devices.

7.  **Investigate unknown devices**  
    Any unexpected device discovered during network scanning should be investigated and verified.

8.  **Maintain network documentation**  
    Network topology and device information should be documented and updated regularly.

9.  **Perform security testing with authorization**  
    Reconnaissance and scanning should only be performed against systems and networks where appropriate authorization has been provided.


## 7. Conclusion

During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

In the footprinting activity, I used six Kali Linux tools to collect information about the target domain. I learned how WHOIS can provide domain information, WhatWeb can identify web technologies, Nslookup can resolve domain names, Curl can inspect HTTP headers, Wafw00f can identify a WAF, and DNSRecon can provide additional DNS information.

In the network scanning activity, I used Zenmap to identify my local network configuration and discover active hosts. I also collected IP and MAC address information and created a network topology.

The exercises showed me that information gathering is an important part of cybersecurity. Even before attempting to exploit a system, a security professional can learn a significant amount about an environment by carefully analyzing publicly available information and network responses.

I also learned that technical findings should be documented clearly. A good cybersecurity report should explain what was performed, what was discovered, what the observation means, what risk it may create, and what can be done to reduce that risk.

Finally, I learned that reconnaissance and scanning must always be performed within an authorized scope. These activities were completed as part of the assigned educational cybersecurity lab.


## 8. Evidences Collected

*Screenshots collected as evidence during the activities (stored in the `screenshots/` folder):* 
<br>

<p align="center">
  <img src="screenshots/1_whois.png" width="800">
  <br>
  <em>Figure 01: WHOIS Domain Information Lookup</em>
</p><br>

<p align="center">
  <img src="screenshots/2_whatweb.png" width="800">
  <br>
  <em>Figure 02: Website Technology Identification Using WhatWeb</em>
</p><br>

<p align="center">
  <img src="screenshots/3_nslookup.png" width="800">
  <br>
  <em>Figure 03: DNS Lookup and Domain Resolution Using Nslookup</em>
</p><br>

<p align="center">
  <img src="screenshots/4_curl.png" width="800">
  <br>
  <em>Figure 04: HTTP Response and Website Information Using cURL</em>
</p><br>

<p align="center">
  <img src="screenshots/5_wafw00f.png" width="800">
  <br>
  <em>Figure 05: Web Application Firewall Detection Using WAFW00F</em>
</p><br>

<p align="center">
  <img src="screenshots/6_dnsrecon.png" width="800">
  <br>
  <em>Figure 06: DNS Enumeration Using DNSRecon</em>
</p><br>

<p align="center">
  <img src="screenshots/7_zenmap.png" width="800">
  <br>
  <em>Figure 07: Network Host Discovery Using Zenmap</em>
</p><br>

<p align="center">
  <img src="screenshots/8_zenmap2.png" width="800">
  <br>
  <em>Figure 08: Network Topology Visualization in Zenmap</em>
</p><br>

<div align="center">

-End of Report-

</div>
<br>

# 👤 Author

**Izzath Inayathullah**<br>
Cybersecurity Enthusiast B084

GitHub: [IzzathInayathullah](https://github.com/IzzathInayathullah) <br>
LinkedIn: [IzzathInayathullah](https://www.linkedin.com/in/izzathinayathullah/)


# 📌 Project Information

- **Project:** Cybersecurity Footprinting/Reconnaissance and Network Scanning
- **Repository:** `NetworkWalks-Izzath-B084-WK2-PM1andPM5-CyberSecurity-Footprinting-and-Network-Scanning-Report`

This project demonstrates practical cybersecurity footprinting, reconnaissance, and network scanning techniques to gather information about target systems, identify active hosts, discover open ports and services, and assess potential security risks within a controlled lab environment.
