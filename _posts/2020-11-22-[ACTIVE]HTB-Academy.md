# [](#header-1)Info card :

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaTMx2RHsiIKIFOwQ8%2Fimage.png?alt=media&token=1ff09c07-12ba-434a-b00e-38686f77bf2b)


# [](#header-1)Enumeration
First things first we should start from the enumeration task by using Nmap or Autorecon something like that (whatever you want to use.)

```
nmap -sC -sV -Pn -oA nmap/academy 10.10.10.215
```

## [](#header-2)Nmap scanned result :

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaUUsjz6eXcWU8RR9L%2Fimage.png?alt=media&token=ddcc8747-d64a-4170-b511-d2da114d0dbf)

From the Nmap result, it tells us in this box has used  2 port :
* 22 SSH 
* 80 HTTP

So we should enumerate with port 80 first.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaXebmJgGPad3-Sx5t%2Fimage.png?alt=media&token=bf91233c-83cf-46b0-9680-77c4536f646e)

you would see this IP address has redirect to <b>academy.htb</b> , So we should add to this virtual host with target an IP address

you can use this command below:
> sudo gedit /etc/hosts

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaYN7EmCgi4vUjocGB%2Fimage.png?alt=media&token=df54e3f7-767b-448e-a6b3-a76bd176dda9)

Okay, we can access this website.

Now we should find a hidden directory or misconfiguration on this website. you can use tools such as dirbuster , gobuster, dirsearch etc., For me I preferred <b>dirseach</b>

> sudo dirsearch -u http://academy.htb/ -t 100 -e * | tee dirscan.txt

## [](#header-2)Dirsearch scanned result :

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMa_QD-Rrebo9MUsAT5%2Fimage.png?alt=media&token=de76c670-ba18-4084-ad79-88962fe97f2f)

We would see interesting stuff such as  <b>/admin.php , /config.php </b> but after I spend time for a while it' doesn't have anything information, Well we should roll back to <b>register.php</b>

* /register.php

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaaVUJruMeNuo4LXNI%2Fimage.png?alt=media&token=574998be-f36f-429f-8b0d-b4cd52c5e563)

Ahha ! We found some interesting.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMaaoEMJnYo1YQr7PUF%2Fimage.png?alt=media&token=992b2d1c-90cd-4d1b-9a7c-ee5c71f589e4)

We should take a look at this source code we will see  hidden input about roleid.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaS_hV9oG02Gbku0-8%2F-MMacPTOsa-FSzdo8GnF%2Fimage.png?alt=media&token=5c914757-102c-49bd-b414-2a9741f241f9)

What! Why return to this login page, why we cannot enter the page. I think roleid it's weird. why it sends attach from this request?

If this variable is boolean type? return TRUE /FALSE, so we would prove it my theory is right just change value from 0 to 1.

