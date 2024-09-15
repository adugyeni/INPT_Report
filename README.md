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


#### Service Discovery And Port Scanning
Service discovery and port scanning are essential techniques in network management, security, and administration.

### Service Discovery
- **Purpose:** Identifies active services on a network, such as web servers or databases.
- **Benefits:**
  - **Configuration Management:** Helps administrators verify and manage service configurations.
  - **Troubleshooting:** Assists in diagnosing and resolving network issues by revealing which services are running.
  - **Security:** Aids in identifying potential vulnerabilities by showing what services are exposed and their current state.

#### Port Scanning
- **Purpose:** Detects open ports on networked devices, revealing which services are available.
- **Benefits:**
  - **Network Mapping:** Helps in understanding the network layout and available services.
  - **Security Assessment:** Identifies open ports and services that may be vulnerable, aiding in vulnerability management.
  - **Compliance:** Ensures systems meet security policies and standards by regularly checking for unintended open ports.

#### Combined Use
- **Enhanced Security:** Together, these techniques provide a comprehensive view of network security, identifying vulnerabilities and ensuring proper configuration.
- **Efficient Management:** They help in maintaining a secure and well-managed network environment by detecting potential issues early.
  
![Service Discovery and Port Scanning](Images/port_scanning_and_service_discovery.png)



  



