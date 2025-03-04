# Penetration Testing Report

**Date:** March 4, 2025  
**Target:** 10.10.146.30 (Lian_Yu)  
**Penetration Tester:** zghost  
**Objective:** Enumerate, exploit, and gain unauthorized access to the target system for assessment.

## Executive Summary

This report documents the enumeration, exploitation, and privilege escalation activities performed on the target system located at IP address 10.10.146.30, codenamed "Lian_Yu." The assessment involved network scanning, web directory enumeration, FTP access, steganalysis, and privilege escalation to retrieve sensitive files (user.txt and root.txt). Key findings include open ports, hidden web content, a successful FTP login with credentials, steganography-based file extraction, and root access via privilege escalation. The target is a Linux-based system running Apache, FTP, and SSH services.

## Methodology

The following tools and techniques were used during the penetration test:

- **Nmap:** To identify open ports and services.
- **Dirb and Gobuster:** For web directory enumeration and file discovery.
- **FTP Client:** To test anonymous and credentialed access.
- **Steghide and Stegseek:** For steganography analysis of image files.
- **View Source:** To inspect web page source code for hidden information.
- **Sudo and pkexec:** For privilege escalation.

## Findings

### 1. Network Enumeration

- **Nmap Scan Results:**  
    An Nmap scan revealed the following open ports and services:
    
    - Port 21/tcp: FTP (vsFTPd 3.0.2)
    - Port 22/tcp: SSH (OpenSSH 6.7p1 Debian 4+deb8u8, protocol 2.0)
    - Port 80/tcp: HTTP (Apache)
    - Port 111/tcp: RPCbind
    - Port 10002/tcp: Status (RPC #100002)
    - Port 38118/tcp: Status (RPC #100024)
    - Operating System: Linux 5.4 (kernel 5.4)
    
    No additional vulnerabilities were identified in the initial scan.
    

### 2. Web Enumeration

- **Initial Web Directory Scan (Dirb):**
    - Using dirb with the wordlist /usr/share/dirb/wordlists/common.txt, no significant directories or files were found.
- **Gobuster Directory Enumeration:**
    - Using gobuster with the wordlist /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt, the following was discovered:
        - Directory: /island (Status: 301, Size: 235)
    - Further enumeration on /island revealed:
        - File: .ticket in the /2100 subdirectory (Status: 200)
        - File: /green_arrow.ticket (Status: 200)
- **Hidden Information in Web Source:**
    - Viewing the source code of http://10.10.146.30/island/ revealed a hidden message and code word:
        - Message: "Ohhh Noo, Don’t Talk... I wasn’t Expecting You at this Moment. I will meet you there. You should find a way to Lian_Yu as we are planed. The Code Word is: vigilant"
        - Code Word: vigilante

### 3. FTP Enumeration and Access

- **Anonymous FTP Access Attempt:**
    - Attempted anonymous FTP login to ftp://10.10.146.30.
    - Result: Permission denied (login failed).
- **Credentialed FTP Access:**
    - Through web enumeration, the string RTy8yhBQdscX was identified in the /green_arrow.ticket file, initially thought to be an encoded password.
    - Further analysis revealed the password was base58-encoded as !#th3h00d.
    - Successfully logged into FTP using credentials {vigilante:!#th3h00d}.
    - Retrieved the following files:
        - Leave_me_alone.png (511,720 bytes)
        - Queen's_Gambit.png (549,924 bytes)
        - aa.jpg (191,026 bytes)

### 4. Steganography Analysis

- **Tools Used:** steghide and stegseek with the /usr/share/wordlists/rockyou.txt wordlist.
- **Findings on aa.jpg:**
    - Using stegseek, a hidden file ss.zip was extracted with the passphrase "password."
    - Unzipping ss.zip revealed:
        - passwd.txt: Contains the message, "This is your visa to Land on Lian_Yu # Just for Fun ***" and a note about Oliver’s resourcefulness on the island, including animals like pheasants, wild pigs, and wolves.
        - shado: Contains the string "M3tahuman," interpreted as a potential username or keyword.

### 5. SSH Access and Privilege Escalation

- **SSH Login:**
    - Using the shado file content ("M3tahuman") and context clues, attempted SSH login as slade with the password M3tahuman.
    - Successfully logged in as slade@LianYu.
- **File Retrieval:**
    - Found and retrieved user.txt in the home directory of slade:
        - Content: THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T} --Felicity Smoak
- **Privilege Escalation:**
    - Ran sudo -l to check sudo privileges for slade:
        - slade can run /usr/bin/pkexec as root with a password prompt.
    - Used sudo pkexec /bin/sh to escalate to root access.
    - Retrieved root.txt in the root directory:
        - Content: Mission accomplished. You are injected me with Mirakuru:) ---> Now slade Will become DEATHSTROKE. THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D} --DEATHSTROKE

### 6. File Discovery Summary

- **Web Files Found:**
    - index.html (Status: 200, Size: 2506)
    - server-status (Status: 403, Size: 199)
    - .ticket (in /2100, Status: 200)
    - /green_arrow.ticket (Status: 200)
- **FTP Files Found:**
    - Leave_me_alone.png
    - Queen's_Gambit.png
    - aa.jpg
- **System Files Found:**
    - user.txt
    - root.txt
    - passwd.txt (from steganography)
    - shado (from steganography)

---

## Recommendations

1. **Further Enumeration:**
    - Conduct a deeper analysis of remaining files (Leave_me_alone.png and Queen's_Gambit.png) for additional hidden data using steganography tools.
2. **Vulnerability Assessment:**
    - Investigate known vulnerabilities in vsFTPd 3.0.2, OpenSSH 6.7p1, and Apache on Linux 5.4 for potential exploits.
    - Assess the security of the pkexec privilege escalation path to prevent unauthorized root access.
3. **Secure the System:**
    - Disable anonymous FTP and restrict FTP/SSH access to authorized users only.
    - Harden the Apache configuration and remove unnecessary files or directories (e.g., .ticket, /green_arrow.ticket).
    - Implement strong password policies and remove or secure steganographically hidden data.

---

## Conclusion

The target system at 10.10.146.30 (Lian_Yu) was successfully compromised through a combination of web enumeration, FTP credential discovery, steganography analysis, and privilege escalation. The hidden code word vigilante, FTP credentials {vigilante:!#th3h00d}, and steganographic extraction of passwd.txt and shado enabled access as slade, ultimately leading to root access and retrieval of user.txt and root.txt. The system’s exposure of sensitive data via steganography and weak privilege controls highlights significant security risks. Further hardening and monitoring are recommended to prevent unauthorized access.