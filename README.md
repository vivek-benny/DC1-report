# DC-1 VulnHub Penetration Test

Author: Vivek Benny
Program: Bachelor of Computer Applications (BCA)
Institution: Marian College Kuttikkanam Autonomous
Year: 2026

--------------------------------------------------

Overview

This repository documents a complete penetration testing assessment of the DC-1 VulnHub virtual machine performed in a controlled laboratory environment.

The objective of the project was to identify security vulnerabilities, exploit exposed services, escalate privileges, and obtain root-level access to the target system.

The project follows the standard penetration testing lifecycle:

Reconnaissance → Enumeration → Exploitation → Post-Exploitation → Privilege Escalation → Root Access

--------------------------------------------------

Lab Architecture

Attacker Machine
Kali Linux

Target Machine
DC-1 VulnHub (Debian Linux)

Virtualization Platform
Oracle VirtualBox

Network Configuration
Host-Only Network

--------------------------------------------------

Tools Used

Nmap – Network scanning and service enumeration  
Metasploit Framework – Exploitation framework  
Linux Command Line – System enumeration and privilege escalation  
Python PTY – Shell stabilization  
VirtualBox – Lab virtualization  

--------------------------------------------------

Attack Methodology

1. Reconnaissance

Network discovery was performed to identify live hosts.

nmap -sP 192.168.130.0/24

The DC-1 target machine was identified.

--------------------------------------------------

2. Enumeration

Service and version detection revealed exposed services.

nmap -sC -sV -O -p- 192.168.130.7

Open ports discovered:

22 (SSH)  
80 (HTTP – Apache Web Server)  
RPC services  

Further investigation revealed the website was running Drupal 7.

--------------------------------------------------

3. Vulnerability Identification

Drupal 7 versions prior to security patches are vulnerable to:

CVE-2018-7600 — Drupalgeddon2

This vulnerability allows Remote Code Execution (RCE).

--------------------------------------------------

4. Exploitation

The vulnerability was exploited using the Metasploit module:

exploit/unix/webapp/drupal_drupalgeddon2

Configuration:

RHOSTS = Target IP  
LHOST = Attacker IP  

Result:

Meterpreter session opened

--------------------------------------------------

5. Post-Exploitation

System information was collected using:

sysinfo

A shell was spawned and stabilized using:

python -c 'import pty; pty.spawn("/bin/sh")'

--------------------------------------------------

6. Privilege Escalation

SUID binaries were enumerated using:

find / -user root -perm /4000 2>/dev/null

The /usr/bin/find binary was exploited:

find . -exec /bin/sh -quit

Result:

root access obtained

--------------------------------------------------

7. Flag Retrieval

After gaining root privileges, the final flag was retrieved from the root directory confirming full system compromise.

--------------------------------------------------

Key Findings

- Outdated Drupal CMS allowed remote code execution
- Publicly exposed services increased attack surface
- Misconfigured SUID binaries enabled privilege escalation
- Lack of patch management led to full compromise

--------------------------------------------------

Repository Structure

dc1-vulnhub-pentest
│
├── README.md
├── report/
├── diagrams/
├── logs/
├── evidence/
└── scripts/

--------------------------------------------------

Educational Purpose

This project was conducted strictly in a controlled laboratory environment for educational purposes.
No unauthorized systems were targeted.

--------------------------------------------------

References

Drupal Security Advisory – CVE-2018-7600  
https://www.drupal.org/sa-core-2018-002

Metasploit Documentation  
https://docs.metasploit.com

Nmap Documentation  
https://nmap.org/docs.html

VulnHub – DC-1  
https://www.vulnhub.com/entry/dc-1,292/

--------------------------------------------------

Author

Vivek Benny
BCA Cyber Security
Marian College Kuttikkanam Autonomous
