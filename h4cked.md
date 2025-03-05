# Penetration Testing Report

**Date:** March 5, 2025  
**Target:** 10.10.169.82 (wir3)  
**Penetration Tester:** zghost  
**Objective:** Replicate an anonymous attacker’s steps to gain unauthorized access to the target system and retrieve the flag.txt file located in the /root/Reptile directory, as part of a penetration testing challenge.

## Executive Summary

This report documents the enumeration, exploitation, and privilege escalation activities performed on the target system located at IP address 10.10.169.82, with the hostname "wir3." The assessment was initiated to replicate the steps of an anonymous threat actor, as evidenced by a provided .pcap file analyzed with Wireshark. The penetration test involved FTP bruteforcing, web shell exploitation, reverse shell establishment, privilege escalation, and file retrieval to access flag.txt. Key findings include successful FTP login, upload of a backdoor (shell.php), reverse shell access as www-data, escalation to jenny and root privileges, and retrieval of the flag. The target is a Linux-based system running Ubuntu 18.04 LTS with FTP, HTTP, and SSH services exposed.

## Methodology

The following tools and techniques were used during the penetration test:

- **Wireshark:** To analyze the .pcap file and determine attacker actions.
- **Hydra:** For password bruteforcing on the FTP service.
- **FTP Client:** To log in, retrieve, and upload files.
- **Netcat (nc):** To create a listener and establish a reverse shell.
- **Python3:** To spawn a TTY shell for improved interaction.
- **Sudo:** For privilege escalation to root access.

## Findings

### 1. Analysis of .pcap File (Wireshark)

- The .pcap file revealed the attacker’s actions, including:
    - Attempting to log into the FTP service (port 21).
    - Using Hydra to brute-force FTP credentials.
    - Uploading a backdoor (shell.php) to the web server.
    - Executing commands to gain a reverse shell, spawn a TTY, escalate privileges, and download a GitHub project.
- Specific details from the .pcap analysis:
    - **Service Targeted:** FTP
    - **Bruteforce Tool:** Hydra (by Van Hauser)
    - **Username Attempted:** jenny
    - **Password Discovered:** password123 (initially, later changed to 987654321)
    - **FTP Working Directory After Login:** /var/www/html
    - **Backdoor Filename:** shell.php
    - **Backdoor Source URL:** http://pentestmonkey.net/tools/php-reverse-shell
    - **Commands Executed After Reverse Shell:**
        - whoami (identified user as www-data)
    - **Computer Hostname:** wir3
    - **TTY Shell Command:** python3 -c 'import pty; pty.spawn("/bin/bash")'
    - **Root Shell Command:** sudo su
    - **GitHub Project Downloaded:** Reptile (used to install a stealthy backdoor, identified as a rootkit)

### 2. Replication of Attacker Steps

#### a. FTP Bruteforcing

- Used hydra -l jenny -P /usr/share/wordlists/rockyou.txt 10.10.169.82 ftp to bruteforce FTP credentials.
- Successfully identified credentials:
    - Username: jenny
    - Password: 987654321 (updated from the original password123 in the .pcap).
- Logged into FTP using ftp jenny@10.10.169.82 with the password 987654321.
- Confirmed FTP working directory: /var/www/html (via ls command).
- Retrieved existing files:
    - index.html (10,918 bytes)
    - shell.php (5,493 bytes, the backdoor).

#### b. Backdoor Exploitation

- Downloaded and modified shell.php (from http://pentestmonkey.net/tools/php-reverse-shell) to include a reverse shell payload targeting port 9001 on the attacker machine.
- Uploaded the modified shell.php to /var/www/html via FTP.
- Created a listener on the attacker machine using nc -lnvp 9001.
- Accessed http://10.10.169.82/shell.php to execute the backdoor, establishing a reverse shell as www-data@wir3.

#### c. TTY Shell and Privilege Escalation

- Spawned a TTY shell using python3 -c 'import pty; pty.spawn("/bin/bash")' for better interaction.
- Identified the user as www-data via whoami.
- Switched to user jenny using su jenny with the password 987654321.
- Checked sudo privileges with sudo -l:
    - jenny has full root privileges ((ALL : ALL) ALL).
- Escalated to root using sudo su.

#### d. Flag Retrieval

- Navigated to /root/Reptile and listed files:
    - Found flag.txt among other project files (e.g., configs, Kconfig, Makefile, README.md, userland, kernel, output, scripts).
- Retrieved flag.txt content:
    - ebcefd66ca4b559d17b440b6e67fd0fd

#### e. Backdoor Analysis

- The attacker downloaded the Reptile project from GitHub, which installs a stealthy backdoor described as "very hard to detect."
- This type of backdoor is identified as a **rootkit**, operating at a low level to maintain persistent, covert access to the system.

### 3. File Discovery Summary

- **FTP Files Found:**
    - index.html (10,918 bytes)
    - shell.php (5,493 bytes, backdoor)
- **System Files Found:**
    - flag.txt (in /root/Reptile)

---

## Recommendations

1. **Further Enumeration:**
    - Conduct a deeper analysis of other files in /root/Reptile (e.g., configs, kernel, userland) for additional malicious functionality or credentials.
    - Perform a comprehensive network and system scan to identify any remaining backdoors or rootkits installed by the Reptile project.
2. **Vulnerability Assessment:**
    - Investigate known vulnerabilities in vsFTPd, Apache httpd, and Ubuntu 18.04 LTS (Linux 4.15.0-135-generic) for potential exploits.
    - Assess the security of the FTP service configuration, as it allows anonymous or weak credential access.
    - Evaluate the sudo privileges for jenny to prevent unauthorized root access in the future.
3. **Secure the System:**
    - Disable or restrict FTP access, requiring strong authentication and disabling anonymous login.
    - Harden the web server (Apache) by removing or securing uploaded files (e.g., shell.php) and monitoring for unauthorized uploads.
    - Remove the Reptile rootkit and scan for any residual malicious activity using rootkit detection tools (e.g., chkrootkit, rkhunter).
    - Implement strong password policies, monitor for brute-force attempts, and enforce network-level security (e.g., firewalls, IDS/IPS).

---

## Conclusion

The target system at 10.10.169.82 (wir3) was successfully compromised by replicating the anonymous attacker’s steps, as detailed in the .pcap file. FTP bruteforcing with Hydra revealed credentials for jenny (987654321), allowing access to upload a backdoor (shell.php) and establish a reverse shell as www-data. Privilege escalation to jenny and root via sudo privileges enabled retrieval of flag.txt (ebcefd66ca4b559d17b440b6e67fd0fd) from /root/Reptile. The attacker’s use of the Reptile GitHub project installed a stealthy rootkit, posing a significant security risk. The system’s exposure of FTP, weak credentials, and misconfigured privileges highlights critical vulnerabilities requiring immediate remediation.