### **Penetration Test Report: Spice Hut**

**Date**: March 2, 2025

**Subject**: Security Assessment of Spice Hut's Infrastructure

**Client**: Spice Hut

**Objective**: Perform a comprehensive penetration test to identify potential security vulnerabilities and attempt to gain root access.

---

### **1. Executive Summary**

Spice Hut is a new startup company with concerns about its security posture. After conducting a thorough penetration test, I have identified several vulnerabilities, including open ports, weak configurations, and exploitable services that allowed for privilege escalation and ultimately, root access.

During the test, I successfully obtained the secret spicy soup recipe, user.txt contents, and root.txt contents. A full detailed report with the findings and exploitation steps is provided below.

---

### **2. Initial Reconnaissance**

**2.1 Nmap Scan Results**

The initial Nmap scan revealed the following open ports and services:

- **Port 21 (FTP)**: vsftpd 3.0.3
    
    - Anonymous FTP login allowed.
    - Writable directory `/ftp` present with a file `important.jpg` and `notice.txt`.
- **Port 22 (SSH)**: OpenSSH 7.2p2
    
    - SSH accessible, no immediate vulnerabilities identified here, but it will be checked after gaining initial access.
- **Port 80 (HTTP)**: Apache 2.4.18
    
    - Web server shows "Maintenance" page.
    - Accessible directories and files found under `/files/` on the web server.

**2.2 DIRB Scan Results**

The DIRB scan identified the following:

- **http://10.10.106.253/files/**: A directory listing is available and open to the public.
- **http://10.10.106.253/index.html**: Found and accessible with a 200 OK response.
- **http://10.10.106.253/server-status**: This page returned a 403 Forbidden error.

The `/files/` directory was listed, and further exploration was performed in this area.

---

### **3. Exploitation Phase**

**3.1 FTP Access**

Anonymous FTP login was allowed, which allowed me to access and upload files to the server. Upon entering the FTP directory, I found the following files:

- `important.jpg` (file size 251631 bytes)
- `notice.txt` (file size 208 bytes)

I proceeded to upload a reverse shell script that would later be triggered from the web server.

**3.2 Web Server Exploitation**

I explored the `/files/` directory and noticed it was **listable**, meaning that an attacker could easily list files and upload malicious content. This directory was key in uploading a malicious script, which I later used for remote access.

**3.3 Reverse Shell Setup**

- I uploaded a reverse shell script to the `/files/` directory.
- The reverse shell was designed to connect back to my local Netcat listener.

**3.4 Gaining Access to the Server**

Once the script was uploaded, I triggered it through the web server. The server executed the script, and I successfully connected to the system via Netcat. I had gained initial access as a low-privileged user (`lennie`).

---

### **4. Privilege Escalation**

**4.1 Investigating User Directories**

Upon gaining access as `lennie`, I started to enumerate the system. I found the directory `/scripts` and the presence of a script `/etc/print.sh` that was running commands on the system.

- I modified `/etc/print.sh` to execute a reverse shell as root.

**4.2 Root Access**

After triggering the modified script, I successfully escalated my privileges to the root user, allowing me to access the root shell.

---

### **5. Findings**

- **Secret Spicy Soup Recipe**: I obtained the recipe from the server, which was stored in one of the files found during the exploration phase.
    
- **User Flag**: The contents of `user.txt` were successfully retrieved once I gained access as `lennie`.
    
- **Root Flag**: The contents of `root.txt` were retrieved after achieving root access.
    

---

### **6. Conclusion and Recommendations**

**Vulnerabilities Identified**:

1. **Anonymous FTP Login**: Allowed unauthorized users to upload files, leading to initial compromise.
2. **Writable Web Directories**: The `/files/` directory was listable and allowed for the uploading of malicious files, which were executed by the server.
3. **Insecure Scripting**: The `/etc/print.sh` script was exploitable, allowing privilege escalation.

**Recommendations**:

1. **Disable Anonymous FTP**: It is recommended to disable anonymous FTP login and restrict FTP access to authenticated users.
2. **Secure Web Directories**: Ensure that sensitive directories, such as `/files/`, are properly secured with appropriate permissions. Consider using `.htaccess` or other mechanisms to prevent public access to these directories.
3. **Review and Secure Scripts**: Scripts like `/etc/print.sh` should be reviewed to ensure that they cannot be exploited for privilege escalation. Proper validation and least-privilege access controls should be enforced.

**Next Steps**:

- Apply security patches and best practices to harden the server.
- Conduct further security assessments to ensure no other vulnerabilities exist.

This report serves as a foundational step in addressing the security concerns at Spice Hut. Further testing and mitigation strategies are advised.