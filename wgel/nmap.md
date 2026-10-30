 sudo nmap 10.10.198.217 -sCV -vvv -A -oN nmapscan.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-28 20:59 EST
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:59
Completed NSE at 20:59, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:59
Completed NSE at 20:59, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:59
Completed NSE at 20:59, 0.00s elapsed
Initiating Ping Scan at 20:59
Scanning 10.10.198.217 [4 ports]
Completed Ping Scan at 20:59, 0.13s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:59
Completed Parallel DNS resolution of 1 host. at 20:59, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 5, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 20:59
Scanning 10.10.198.217 (10.10.198.217) [1000 ports]
Discovered open port 22/tcp on 10.10.198.217
Discovered open port 80/tcp on 10.10.198.217
Completed SYN Stealth Scan at 20:59, 2.10s elapsed (1000 total ports)
Initiating Service scan at 20:59
Scanning 2 services on 10.10.198.217 (10.10.198.217)
Completed Service scan at 20:59, 6.32s elapsed (2 services on 1 host)
Initiating OS detection (try #1) against 10.10.198.217 (10.10.198.217)
Retrying OS detection (try #2) against 10.10.198.217 (10.10.198.217)
Retrying OS detection (try #3) against 10.10.198.217 (10.10.198.217)
Retrying OS detection (try #4) against 10.10.198.217 (10.10.198.217)
Retrying OS detection (try #5) against 10.10.198.217 (10.10.198.217)
Initiating Traceroute at 21:00
Completed Traceroute at 21:00, 0.08s elapsed
Initiating Parallel DNS resolution of 1 host. at 21:00
Completed Parallel DNS resolution of 1 host. at 21:00, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 5, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
NSE: Script scanning 10.10.198.217.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 2.46s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 0.31s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 0.00s elapsed
Nmap scan report for 10.10.198.217 (10.10.198.217)
Host is up, received echo-reply ttl 63 (0.073s latency).
Scanned at 2025-02-28 20:59:46 EST for 25s
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpgV7/18RfM9BJUBOcZI/eIARrxAgEeD062pw9L24Ulo5LbBeuFIv7hfRWE/kWUWdqHf082nfWKImTAHVMCeJudQbKtL1SBJYwdNo6QCQyHkHXslVb9CV1Ck3wgcje8zLbrml7OYpwBlumLVo2StfonQUKjfsKHhR+idd3/P5V3abActQLU8zB0a4m3TbsrZ9Hhs/QIjgsEdPsQEjCzvPHhTQCEywIpd/GGDXqfNPB0Yl/dQghTALyvf71EtmaX/fsPYTiCGDQAOYy3RvOitHQCf4XVvqEsgzLnUbqISGugF8ajO5iiY2GiZUUWVn4MVV1jVhfQ0kC3ybNrQvaVcXd
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDCxodQaK+2npyk3RZ1Z6S88i6lZp2kVWS6/f955mcgkYRrV1IMAVQ+jRd5sOKvoK8rflUPajKc9vY5Yhk2mPj8=
|   256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJhXt+ZEjzJRbb2rVnXOzdp5kDKb11LfddnkcyURkYke
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=2/28%OT=22%CT=1%CU=43889%PV=Y%DS=2%DC=T%G=Y%TM=67C2
OS:6A2B%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=10D%TI=Z%CI=I%II=I%TS=A)
OS:SEQ(SP=106%GCD=2%ISR=10D%TI=Z%CI=I%II=I%TS=A)OPS(O1=M508ST11NW6%O2=M508S
OS:T11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST11NW6%O6=M508ST11)WIN(W1=
OS:68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=
OS:M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)
OS:T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S
OS:+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=
OS:Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G
OS:%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Uptime guess: 4.606 days (since Mon Feb 24 06:27:16 2025)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=262 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 53/tcp)
HOP RTT      ADDRESS
1   72.52 ms 10.8.0.1 (10.8.0.1)
2   72.12 ms 10.10.198.217 (10.10.198.217)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:00
Completed NSE at 21:00, 0.00s elapsed
Read data files from: /usr/share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.10 seconds
           Raw packets sent: 1125 (53.526KB) | Rcvd: 1081 (46.718KB)
