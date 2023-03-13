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

