# Penetration Testing Report

## Executive Summary

This penetration test, conducted by \[zghost\] on September 18, 2025, targeted a single host at IP address `10.10.24.25` as part of a TryHackMe (THM) lab exercise named "Agent-T." The objective was to identify vulnerabilities, gain unauthorized access, and retrieve sensitive data (e.g., a flag) to assess the security posture of the target system.

Key findings include:

- **Critical Vulnerability**: A PHP backdoor (version 8.1.0-dev) on port 80, allowing remote code execution (RCE) due to a known vulnerability (likely CVE-2021-42013).
- **High-Risk Issue**: Exposure of a sensitive file (`flag.txt`) in the root directory, accessible without proper access controls.
- **Other Observations**: The system runs an outdated Linux kernel (4.X or 5.X), potentially vulnerable to privilege escalation exploits.
- **Recommendations**: Immediate patching of the PHP backdoor, restricting file system access, and implementing network segmentation to limit exposure.

The test was conducted in a black-box manner, simulating an external attacker with no prior knowledge of the system.

## Scope

The penetration test focused on the following:

- **Target Host**: `10.10.24.25`
- **Services**: HTTP service (PHP CLI server) on port 80
- **Testing Period**: September 18, 2025, 08:24:22 - 08:28:17 EEST
- **Testing Type**: Black-box, external network and application testing
- **Limitations**: Testing was confined to the provided IP address within the THM lab environment. No social engineering or physical access was attempted.

## Methodology

The penetration test adhered to industry-standard methodologies (OWASP, PTES) and followed these steps:

1. **Reconnaissance**: Used Nmap to enumerate open ports, services, and operating system details.
2. **Vulnerability Scanning**: Identified a PHP CLI server (8.1.0-dev) with a potential backdoor vulnerability.
3. **Exploitation**: Executed a Python script (`49933.py`) to exploit the PHP backdoor, gaining remote code execution.
4. **Post-Exploitation**: Searched for sensitive files (`flag.txt`) and retrieved its contents.
5. **Reporting**: Documented findings with evidence and provided remediation recommendations.

Testing was conducted via a VPN (likely TryHackMe's), with traceroute indicating a network distance of 2 hops.

## Findings

The following vulnerabilities and issues were identified, categorized by severity.

### Critical Severity

#### 1. PHP Backdoor Allowing Remote Code Execution

- **Description**: The HTTP service on port 80 runs PHP CLI server 8.1.0-dev, which is vulnerable to a known backdoor (likely CVE-2021-42013). The Python script (`49933.py`) exploited this vulnerability, allowing arbitrary command execution.
- **Impact**: Attackers can execute arbitrary commands, potentially leading to full system compromise, data exfiltration, or malware deployment.
- **Evidence**:
  - Nmap output: `80/tcp open http PHP cli server 5.5 or later (PHP 8.1.0-dev)`.
  - Successful execution of `python3 49933.py`, resulting in a shell.
  - Error message indicating a `system()` function exploit: `Fatal error: Uncaught ValueError: system(): Argument #1 ($command) cannot be empty`.
- **CVSS Score**: 9.8 (Critical)
- **Recommendation**:
  - Upgrade PHP to a stable, patched version (e.g., 8.1.0 or later, non-dev).
  - Disable the PHP CLI server in production environments.
  - Implement a Web Application Firewall (WAF) to filter malicious requests.
  - Conduct a forensic analysis to ensure no persistent backdoors remain.

### High Severity

#### 2. Sensitive File Exposure in Root Directory

- **Description**: The file `/flag.txt` was found in the root directory and was readable without authentication, exposing sensitive data.
- **Impact**: Unauthorized access to sensitive information (e.g., `flag{4127d0530abf16d6d23973e3df8dbecb}`) could lead to further exploitation or compromise of related systems.
- **Evidence**:
  - Command: `find / -type f -name "flag.txt" 2>/dev/null` located `/flag.txt`.
  - Command: `cat /flag.txt` revealed the contents: `flag{4127d0530abf16d6d23973e3df8dbecb}`.
- **CVSS Score**: 7.5 (High)
- **Recommendation**:
  - Restrict file permissions to prevent unauthorized access (e.g., `chmod 600 /flag.txt`).
  - Move sensitive files to a secure, non-public directory.
  - Implement access controls and audit file system permissions regularly.

### Medium Severity

#### 3. Outdated Linux Kernel

- **Description**: The target runs Linux kernel 4.X or 5.X (detected by Nmap), which may be vulnerable to known privilege escalation exploits (e.g., Dirty COW, CVE-2016-5195).
- **Impact**: An attacker with initial access could escalate privileges to root, gaining full control of the system.
- **Evidence**: Nmap OS detection: `Running: Linux 4.X|5.X`, `OS details: Linux 4.15 - 5.19`.
- **CVSS Score**: 6.5 (Medium)
- **Recommendation**:
  - Update the Linux kernel to the latest stable version.
  - Regularly apply security patches and monitor for CVEs.
  - Harden the system by disabling unnecessary kernel modules.

### Low Severity

#### 4. Predictable TCP Sequence Numbers

- **Description**: Nmap reported a TCP sequence prediction difficulty of 264, indicating predictable sequence numbers, which could aid in TCP spoofing attacks.
- **Impact**: An attacker could potentially hijack TCP sessions, though this requires specific conditions.
- **Evidence**: Nmap output: `TCP Sequence Prediction: Difficulty=264 (Good luck!)`.
- **CVSS Score**: 3.7 (Low)
- **Recommendation**:
  - Configure the system to use randomized TCP sequence numbers.
  - Monitor network traffic for signs of spoofing attempts.

## Recommendations Summary

1. **Patch PHP Backdoor**:
   - Upgrade to a stable PHP version and disable the CLI server.
   - Deploy a WAF to mitigate exploitation attempts.
2. **Secure File System**:
   - Restrict access to sensitive files like `flag.txt`.
   - Audit file permissions and relocate sensitive data.
3. **Update System**:
   - Apply the latest kernel patches and monitor for CVEs.
   - Harden the system to reduce the attack surface.
4. **Network Security**:
   - Implement network segmentation to limit access to critical systems.
   - Use firewalls to restrict traffic to port 80 from untrusted sources.
5. **Security Awareness**:
   - Conduct regular pentests and vulnerability assessments.
   - Train staff on secure coding and system administration practices.

## Conclusion

The penetration test successfully identified critical vulnerabilities in the target system, including a PHP backdoor and exposed sensitive data. By exploiting the PHP vulnerability, a shell was obtained, leading to the discovery of `flag.txt`. Immediate remediation is critical to prevent real-world exploitation. Implementing the recommended measures will significantly improve the security posture of the system.

For further assistance or a follow-up assessment, please contact \[zghost\].

---

**Report Prepared By**: zghost\
**Date**: September 18, 2025