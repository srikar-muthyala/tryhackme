**Basic Pentest**

IP - 10.10.216.90

Recon- 

```
nmap 10.10.216.90
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-11 08:19 EST
Nmap scan report for 10.10.216.90
Host is up (0.18s latency).
Not shown: 995 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
8009/tcp open  ajp13

Nmap done: 1 IP address (1 host up) scanned in 19.93 seconds
```

enum4linux -
```
 =================================( Share Enumeration on 10.10.216.90 )=================================
                                                                                                                    
                                                                                                                    
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

[+] Attempting to map shares on 10.10.216.90                                                                        
                                                                                                                    
//10.10.216.90/Anonymous        Mapping: OK Listing: OK Writing: N/A                                                

[E] Can't understand response:                                                                                      
                                                                                                                    
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*                                                                          
//10.10.216.90/IPC$     Mapping: N/A Listing: N/A Writing: N/A

```
```
smbclient //10.10.216.90/Anonymous
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Apr 19 13:31:20 2018
  ..                                  D        0  Thu Apr 19 13:13:06 2018
  staff.txt                           N      173  Thu Apr 19 13:29:55 2018

                14318640 blocks of size 1024. 10821908 blocks available

```

