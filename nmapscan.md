sudo nmap 10.10.178.215 -sCV -vvv -A -p- -oN nmapscan.txt
[sudo] password for zghost: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-04 21:08 EST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:08
Completed NSE at 21:08, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:08
Completed NSE at 21:08, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:08
Completed NSE at 21:08, 0.00s elapsed
Initiating Ping Scan at 21:08
Scanning 10.10.178.215 [4 ports]
Completed Ping Scan at 21:08, 0.10s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:08
Completed Parallel DNS resolution of 1 host. at 21:08, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 5, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 21:08
Scanning 10.10.178.215 (10.10.178.215) [65535 ports]
Discovered open port 80/tcp on 10.10.178.215
Discovered open port 21/tcp on 10.10.178.215
Discovered open port 22/tcp on 10.10.178.215
SYN Stealth Scan Timing: About 6.28% done; ETC: 21:16 (0:07:42 remaining)
SYN Stealth Scan Timing: About 13.95% done; ETC: 21:15 (0:06:16 remaining)
SYN Stealth Scan Timing: About 18.08% done; ETC: 21:16 (0:06:52 remaining)
SYN Stealth Scan Timing: About 21.67% done; ETC: 21:17 (0:07:17 remaining)
SYN Stealth Scan Timing: About 32.59% done; ETC: 21:18 (0:06:45 remaining)
SYN Stealth Scan Timing: About 43.03% done; ETC: 21:16 (0:04:59 remaining)
SYN Stealth Scan Timing: About 50.75% done; ETC: 21:17 (0:04:29 remaining)
SYN Stealth Scan Timing: About 57.15% done; ETC: 21:17 (0:03:50 remaining)
SYN Stealth Scan Timing: About 65.52% done; ETC: 21:17 (0:03:21 remaining)
SYN Stealth Scan Timing: About 74.94% done; ETC: 21:17 (0:02:18 remaining)
SYN Stealth Scan Timing: About 80.38% done; ETC: 21:17 (0:01:49 remaining)
SYN Stealth Scan Timing: About 91.61% done; ETC: 21:16 (0:00:43 remaining)
Completed SYN Stealth Scan at 21:16, 500.76s elapsed (65535 total ports)
Initiating Service scan at 21:16
Scanning 3 services on 10.10.178.215 (10.10.178.215)
Completed Service scan at 21:16, 6.20s elapsed (3 services on 1 host)
Initiating OS detection (try #1) against 10.10.178.215 (10.10.178.215)
Retrying OS detection (try #2) against 10.10.178.215 (10.10.178.215)
Initiating Traceroute at 21:16
Completed Traceroute at 21:16, 0.08s elapsed
Initiating Parallel DNS resolution of 1 host. at 21:16
Completed Parallel DNS resolution of 1 host. at 21:16, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 5, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
NSE: Script scanning 10.10.178.215.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:16
NSE: [ftp-bounce 10.10.178.215:21] PORT response: 500 Illegal PORT command.
Completed NSE at 21:17, 30.50s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:17
Completed NSE at 21:17, 0.52s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:17
Completed NSE at 21:17, 0.00s elapsed
Nmap scan report for 10.10.178.215 (10.10.178.215)
Host is up, received echo-reply ttl 63 (0.068s latency).
Scanned at 2025-03-04 21:08:09 EST for 542s
Not shown: 55529 filtered tcp ports (no-response), 10003 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.34.46
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgcwCtWTBLYfcPeyDkCNmq6mXb/qZExzWud7PuaWL38rUCUpDu6kvqKMLQRHX4H3vmnPE/YMkQIvmz4KUX4H/aXdw0sX5n9jrennTzkKb/zvqWNlT6zvJBWDDwjv5g9d34cMkE9fUlnn2gbczsmaK6Zo337F40ez1iwU0B39e5XOqhC37vJuqfej6c/C4o5FcYgRqktS/kdcbcm7FJ+fHH9xmUkiGIpvcJu+E4ZMtMQm4bFMTJ58bexLszN0rUn17d2K4+lHsITPVnIxdn9hSc3UomDrWWg+hWknWDcGpzXrQjCajO395PlZ0SBNDdN+B14E0m6lRY9GlyCD9hvwwB
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMCu8L8U5da2RnlmmnGLtYtOy0Km3tMKLqm4dDG+CraYh7kgzgSVNdAjCOSfh3lIq9zdwajW+1q9kbbICVb07ZQ=
|   256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICqmJn+c7Fx6s0k8SCxAJAoJB7pS/RRtWjkaeDftreFw
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
OS fingerprint not ideal because: Didn't receive UDP response. Please try again with -sSU
Aggressive OS guesses: HP P2000 G3 NAS device (89%), Linux 2.6.32 (88%), Linux 2.6.32 - 3.1 (88%), Infomir MAG-250 set-top box (88%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (88%), Linux 3.7 (88%), Linux 5.0 (88%), Linux 5.0 - 5.4 (88%), Linux 5.1 (88%), Ubiquiti AirOS 5.5.9 (88%)
No exact OS matches for host (test conditions non-ideal).
TCP/IP fingerprint:
SCAN(V=7.94SVN%E=4%D=3/4%OT=21%CT=20%CU=%PV=Y%DS=2%DC=T%G=N%TM=67C7B427%P=x86_64-pc-linux-gnu)
SEQ(SP=104%GCD=1%ISR=10B%TI=Z%CI=Z%II=I%TS=A)
OPS(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST11NW6%O6=M508ST11)
WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W6=F4B3)
ECN(R=Y%DF=Y%TG=40%W=F507%O=M508NNSNW6%CC=Y%Q=)
T1(R=Y%DF=Y%TG=40%S=O%A=S+%F=AS%RD=0%Q=)
T2(R=N)
T3(R=N)
T4(R=Y%DF=Y%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T5(R=Y%DF=Y%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
T6(R=Y%DF=Y%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)
T7(R=N)
U1(R=N)
IE(R=Y%DFI=N%TG=40%CD=S)

Uptime guess: 36.898 days (since Sun Jan 26 23:43:49 2025)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 43620/tcp)
HOP RTT      ADDRESS
1   72.98 ms 10.8.0.1 (10.8.0.1)
2   73.48 ms 10.10.178.215 (10.10.178.215)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:17
Completed NSE at 21:17, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:17
Completed NSE at 21:17, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:17
Completed NSE at 21:17, 0.00s elapsed
Read data files from: /usr/share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 542.71 seconds
           Raw packets sent: 288161 (12.683MB) | Rcvd: 16550 (1.913MB)
