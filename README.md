# INTERNAL NETWORK PENETRATION TESTING REPORT
#### BY ADU GYENI BENJAMIN

### Table of Contents
1. [Summary](#summary)
2. [Testing_Methodogy](#TestingMethodogy)
3. [Host_Discovery](#HostDiscovery)
4. [Sevice_Discovery_and_Port_Scanning](#SeviceDiscoveryandPortScanning)
5. [Web-Based_Attack_Surfaces](#Web-BasedAttackSurfaces)
6. [Generating_Payloads](#GeneratingPayloads)

### Summary
An Internal Network Penetration Test was performed on a scope comprising 10.10.10.0/24 and a domain name https://virtualinfosecafrica.com/ from September 12th, 2024 to September 14th, 2024. This report describes penetration testing that represents a point in time snapshots of the network security posture of the scope in question which can be found in the subsequent sessions.


### Testing Methodogy
Testing took off by utilizing the Network Mapper(NMAP) tool to locate live hosts and services in the in-scope ip address provided. The output of this tooling was then manually examined and the host and services were manually enumerated to uncover any missed vulnerability and weakness. Web-based attack surfaces were exploited to generate payloads.


### Host Discovery
Host discovery is a process of finding live hosts on a network. It can be used to narrow down the scope of a network assessment or during security testing. One tool which is mostly used in host discovery is the nmap tool. The command used in the host discovery was " nmap -sn 10.10.10.0/24 ", where " -sn (or --ping-scan) " - tells nmap to perform a "ping scan" which is used to discover hosts without performing a full port scan. It will only check if hosts are alive, not what services they are running.
" 10.10.10.0/24 " - specifies the target network. The /24 is a CIDR notation indicating a subnet mask of 255.255.255.0, which means the scan will cover all IP addresses from 10.10.10.1 to 10.10.10.254.

![Host Discovery](Images/host_discovery.png)

The below image is a description of how subdomain enumeration was performed using aiodnsbrute tool
![Aiodnsbrute](Images/aiodns.png)



### Service Discovery And Port Scanning
Service discovery and port scanning are essential techniques in network management, security, and administration.

##### Service Discovery
- **Purpose:** Identifies active services on a network, such as web servers or databases.
- **Benefits:**
  - **Configuration Management:** Helps administrators verify and manage service configurations.
  - **Troubleshooting:** Assists in diagnosing and resolving network issues by revealing which services are running.
  - **Security:** Aids in identifying potential vulnerabilities by showing what services are exposed and their current state.

##### Port Scanning
- **Purpose:** Detects open ports on networked devices, revealing which services are available.
- **Benefits:**
  - **Network Mapping:** Helps in understanding the network layout and available services.
  - **Security Assessment:** Identifies open ports and services that may be vulnerable, aiding in vulnerability management.
  - **Compliance:** Ensures systems meet security policies and standards by regularly checking for unintended open ports.

##### Combined Use
- **Enhanced Security:** Together, these techniques provide a comprehensive view of network security, identifying vulnerabilities and ensuring proper configuration.
- **Efficient Management:** They help in maintaining a secure and well-managed network environment by detecting potential issues early.
  
![Service Discovery and Port Scanning](Images/sdps.png)


### Vulnerability Scanning
Vulnerability scanning is an automated process that identifies security weaknesses in systems, networks, or applications. It helps detect unpatched software, misconfigurations, and other vulnerabilities that could be exploited. The scanning process involves discovering live hosts, scanning for known issues, analyzing results, and generating reports. 

- The image below shows how a custom wordlist can be created using cewl and also procedure used during the vulnerability scanning:

![Vulnerability Scanning](Images/vulnerability_scanning.png)

---


#### Summary of Findings
| Finding                                        | Severity |
|------------------------------------------------|----------|
| Unauthenticated Remote Code Execution (RCE) | Critical    |
| Denial of service (DoS)                          | Moderate     |
|UltraVNC DSM Plugin Local Privilege Escalation                   | High     |
| Weak Tomcat Authentication and Deployment         | Critical   |
| Arbitrary Code Execution via Base64 Decoding                         | Critical|
| Apache Tomcat AJP File Read/Inclusion |Critical  |

#### Detailed Findings
#### Unauthenticated Remote Code Execution (RCE)

| *Current Rating:* | CVSS Score   |
|-------------------|--------------|
|Critical           |   9.8         |

#### Evidence
This module exploit an unauthenticated RCE vulnerability which 
  exists in Apache version 2.4.49 (CVE-2021-41773). If files outside 
  of the document root are not protected by ‘require all denied’ 
  and CGI has been explicitly enabled, it can be used to execute 
  arbitrary commands (Remote Command Execution). This vulnerability 
  has been reintroduced in Apache 2.4.50 fix (CVE-2021-42013).

  - Output after using metaploit auxilliary module
    ![metasploit 1](Images/metasploit1.png)

    ![metasploit 2](Images/metasploit2.png)

 #### Affected Resources are;
      '10.10.10.2, 10.10.10.30, 10.10.10.45, 10.10.10.55'

 #### Recommendations
      Update to a newer patched version of Apache HTTP Server.

---
### Denial of service (DoS)

| *Current Rating:* | CVSS Score   |
|-------------------|--------------|
|Medium          |   6.5         |

These are the vulnerabilities associated with the service version  `MySQL 5.6.49 ` with the port `3306`

#### Evidence
CVE-2020-14765: This vulnerability exists in the FTS component of MySQL Server. It allows a low-privileged attacker with network access to cause a denial of service (DoS) by causing the MySQL Server to hang or crash. The CVSS 3.1 Base Score for this vulnerability is 6.5, indicating a medium level of severity, primarily affecting availability.

CVE-2020-14769: Found in the Optimizer component of MySQL Server, this vulnerability also allows a low-privileged attacker with network access to potentially cause a hang or crash, leading to a complete DoS of the MySQL Server. This issue also has a CVSS 3.1 Base Score of 6.5, indicating medium severity with an impact on availability.

![msql1](Images/msql1)

![msql2](Images/msql2)


#### Affected Resources:
`10.10.10.5 , 10.10.10.40`

#### Recommendations
* Rate Limiting: Implement rate limiting to control the number of requests a user can make to a service in a given timeframe. This can help mitigate the impact of DoS attacks by limiting the number of requests that can overwhelm the system.

* Traffic Filtering and Shaping: Use firewalls and intrusion prevention systems (IPS) to filter out malicious traffic. Traffic shaping can prioritize legitimate traffic and limit the impact of the attack.

* Load Balancing: Distribute incoming traffic across multiple servers or resources. This can help prevent any single server from being overwhelmed and ensure continuity of service.


---
### UltraVNC DSM Plugin Local Privilege Escalation Vulnerability
| *Current Rating:* | CVSS Score   |
|-------------------|--------------|
|  High           |     7.8      |

It was discovered that the service version for the affected resourses which is UltraVNC 1.2.1.7 is the old version which contain vulnerabilities which could be exploited.

#### Evidence
CVE-2022-24750	UltraVNC is a free and open source remote pc access software. A vulnerability has been found in versions prior to 1.3.8.0 in which the DSM plugin module, which allows a local authenticated user to achieve local privilege escalation (LPE) on a vulnerable system. The vulnerability has been fixed to allow loading of plugins from the installed directory. Affected users should upgrade their UltraVNC to 1.3.8.1. Users unable to upgrade should not install and run UltraVNC server as a service. It is advisable to create a scheduled task on a low privilege account to launch WinVNC.exe instead. There are no known workarounds if winvnc needs to be started as a service.

![VNC 1](Images/vnc1)

![VNC 2](Images/vnc2)


#### Affected resouces:
`10.10.10.50`


#### Recommendation
Upgrade to the latest version preferably version UltraVNC 1.5.0.0





### Web-Based Attack Surfaces
Web-based attack surfaces include web application interfaces, authentication mechanisms, APIs, and server configurations, each posing potential security risks like SQL injection or session hijacking. Key security measures involve input validation, secure session management, and keeping software updated. Addressing vulnerabilities in these areas helps protect against exploits and enhances overall web security.

- To use eyewitness to take screenshots of the servers, including those running on non-standard HTTP/HTTPS ports, you can use the following bash command: "eyewitness -f hosts.txt --web --resolve --ports 80, 443, 8080, 8443"

- To generate a base64-encoded payload that triggers a TCP bind shell on executiion on host 10.10.10.55(Apache Tomcat), use this bash command: "msfvenom -p java/meterpreter/bind-tcp LHOST=10.10.10.55 LPORT=4444 -f jar"

- To generate a payload that triggers a TCP bind shell on executiion on host 10.10.10.30(Python Server), use this bash command: "msfvenom -p python/meterpreter/bind-tcp LHOST=10.10.10.30 LPORT=4444 -e base64"

![web attack surfaces](Images/was1.1.png)


![web attack surfaces](Images/was1.2.png)


![web attack](Images/was2.png)
  



