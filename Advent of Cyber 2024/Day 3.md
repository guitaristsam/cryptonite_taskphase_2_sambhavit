![image](https://github.com/user-attachments/assets/777833e6-4543-4fae-aba0-69fb002e6e2e)


![image](https://github.com/user-attachments/assets/4cf9bc5a-26e3-4767-873c-4ccb73b50472)






![image](https://github.com/user-attachments/assets/e839f7f0-abd9-40d0-afd8-e5ff3ffc14da)

`sudo nano /etc/hosts`

![image](https://github.com/user-attachments/assets/4b63952c-772e-48a3-881e-87e342b037ab)




![image](https://github.com/user-attachments/assets/3ee36662-3e60-4902-97c9-c57fb16fdf72)


![image](https://github.com/user-attachments/assets/b498129f-eae0-496e-be5d-0ab355cc5982)


```
┌──(kali㉿kali)-[~]
└─$ dirsearch -u http://frostypines.thm/ --timeout 30

/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3                                                                                                                                                        
 (_||| _) (/_(_|| (_| )                                                                                                                                                                 
                                                                                                                                                                                        
Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/kali/reports/http_frostypines.thm/__24-12-06_08-10-31.txt

Target: http://frostypines.thm/

[08:10:31] Starting:                                                                                                                                                                    
[08:10:53] 301 -  322B  - /js  ->  http://frostypines.thm/public/js/        
[08:10:53] 403 -  280B  - /%3f/
[08:11:00] 403 -  280B  - /.ht_wsr.txt                                      
[08:11:00] 403 -  280B  - /.htaccess.bak1                                   
[08:11:00] 403 -  280B  - /.htaccess.sample
[08:11:00] 403 -  280B  - /.htaccess.orig
[08:11:00] 403 -  280B  - /.htaccess.save
[08:11:00] 403 -  280B  - /.htaccess_extra                                  
[08:11:00] 403 -  280B  - /.htaccess_orig
[08:11:00] 403 -  280B  - /.htaccess_sc
[08:11:00] 403 -  280B  - /.htaccessBAK
[08:11:00] 403 -  280B  - /.htaccessOLD2
[08:11:00] 403 -  280B  - /.htaccessOLD                                     
[08:11:00] 403 -  280B  - /.htm
[08:11:00] 403 -  280B  - /.html                                            
[08:11:00] 403 -  280B  - /.htpasswd_test                                   
[08:11:00] 403 -  280B  - /.httr-oauth                                      
[08:11:00] 403 -  280B  - /.htpasswds
[08:11:02] 403 -  280B  - /.php                                             
[08:11:11] 200 -    3KB - /aboutus.php                                      
[08:11:12] 200 -    2KB - /accounts.php                                     
[08:11:14] 301 -  325B  - /admin  ->  http://frostypines.thm/public/admin/  
[08:11:15] 200 -    2KB - /admin/                                           
[08:11:17] 200 -    2KB - /admin/index.php                                  
[08:11:41] 200 -    0B  - /config.php                                       
[08:11:44] 301 -  324B  - /core  ->  http://frostypines.thm/public/core/    
[08:11:44] 301 -  323B  - /css  ->  http://frostypines.thm/public/css/      
[08:11:58] 301 -  328B  - /includes  ->  http://frostypines.thm/public/includes/
[08:11:58] 403 -  280B  - /includes/                                        
[08:12:00] 301 -  323B  - /javascript  ->  http://frostypines.thm/javascript/
[08:12:01] 403 -  280B  - /js/                                              
[08:12:04] 200 -    3KB - /login.php                                        
[08:12:07] 301 -  325B  - /media  ->  http://frostypines.thm/public/media/  
[08:12:07] 403 -  280B  - /media/                                           
[08:12:12] 301 -  325B  - /node_modules  ->  http://frostypines.thm/node_modules/
[08:12:15] 302 -   10KB - /payment.php  ->  reservation.php                 
[08:12:17] 301 -  323B  - /phpmyadmin  ->  http://frostypines.thm/phpmyadmin/
[08:12:19] 200 -    3KB - /phpmyadmin/doc/html/index.html                   
[08:12:20] 200 -    3KB - /phpmyadmin/                                      
[08:12:20] 200 -    3KB - /phpmyadmin/index.php
[08:12:22] 301 -  319B  - /public  ->  http://frostypines.thm/public/       
[08:12:22] 302 -  288B  - /Public/  ->  http://frostypines.thm/             
[08:12:22] 302 -  288B  - /public/  ->  http://frostypines.thm/
[08:12:22] 302 -  295B  - /public/storage  ->  http://frostypines.thm/storage
[08:12:22] 302 -  299B  - /public/adminer.php  ->  http://frostypines.thm/adminer.php
[08:12:22] 302 -  294B  - /public/system  ->  http://frostypines.thm/system 
[08:12:22] 302 -  291B  - /public/hot  ->  http://frostypines.thm/hot       
[08:12:28] 403 -  280B  - /server-status                                    
[08:12:28] 403 -  280B  - /server-status/                                   
[08:12:31] 200 -    3KB - /signup.php                                       
[08:12:43] 302 -  291B  - /v1/public/yql  ->  http://frostypines.thm/yql    
[08:12:44] 403 -  280B  - /vendor/                                          
                                                                              
Task Completed 
```

saw admin


![image](https://github.com/user-attachments/assets/891ea52b-f16f-4960-97eb-3b1c1d1b868d)






![image](https://github.com/user-attachments/assets/6313d004-c7e3-443f-ad50-af8b1658723f)



![image](https://github.com/user-attachments/assets/80a4dbad-551d-445b-b1bd-a3bec681dc03)

![image](https://github.com/user-attachments/assets/dea3aee8-279e-493b-bc8e-6c5c15cc85ea)

`http://frostypines.thm/media/images/rooms/shell.php`


Saw that webshell was working and ran `cat flag.txt`.

![image](https://github.com/user-attachments/assets/ec6fcd47-7d12-420a-af19-2c8107437332)


