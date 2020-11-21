# [](#header-1) Summary
This box has fun a lot it's straight forward you should enumerate and enumeration harder.

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

After that I tried to login with credential blackcat.and we can access <b>admin-page.php</b>

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMalPtLbSR14Boupziy%2Fimage.png?alt=media&token=07fad61e-3688-473e-8060-29d345c53990)

We will see Fix issue in the last line. It may be a subdomain? , I add it to hosts file.

And that it's right this is a subdomain. It would show about error and information after I spend for a while I know that this is a Laravel framework. and what's next? If we take a look at this 

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMano9MjcmTvUbP1FLo%2Fimage.png?alt=media&token=b903c7c5-bb60-493f-a36c-c792cd8b64f8)

After I know the software running on the server. I googling with keyword <b>"PHP Laravel exploit"</b> and found PHP rce with metasploit.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MManTXRRccB-goB203m%2Fimage.png?alt=media&token=a1522ce4-3262-4c89-b369-0f3bd1a53a9f)

# [](#header-1)Exploit

We used metasploit and we got a shell.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMaqZN7FZNymJO_AAnO%2Fimage.png?alt=media&token=f9548dd2-4b81-4aaf-bd9b-1915c0901590)

## [](#header-2)Got www-data shell

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMasWs1qLWMxNJTmaim%2Fimage.png?alt=media&token=dc0f0553-23d5-44ac-9b1f-5658ce30f332)

This is not Fully TTY connection, I use this command below to spawn shell :
> python3 -c "import pty; pty.spawn('/bin/bash)"

After that I realize this is a Laravel framework so the credentials should be stored in <b>.env</b> file. You can visit this from below.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMat74gcUJhFM6MVy3g%2Fimage.png?alt=media&token=f155959b-f2ad-478e-96e5-acecc1d73140)

Now we got the password but what is username ? it have 2 way to got the username:
* From website if you remember in admin-page has tell us about username in <b>"Complete initial set of modules" </b>
* You can ensure that you can just go to <b>/home</b> path and you will see user 

### [](#header-3)Got user.txt

Now we got user flag

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMaitG1Qho28jXX6qBe%2F-MMateFB0Xsc--ZFoR9h%2Fimage.png?alt=media&token=8c6b44eb-50fb-45e7-92bb-7afeca95871e)

# [](#header-1)Privilege escalation

Next step I user <b>Linpeas.sh</b> for enum in this phase.

After I spend for a long time in this phase I found an interested file :


![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMcdsCvWOTn-a9cH4WB%2Fimage.png?alt=media&token=89df6d9a-6bb0-4fb3-aba8-923636946a26)

Until I found <b>audit.log</b> file and now let's go.

After I download autdit.log file to My vm I using VScode to find some keyword or credential ,and I found something :
![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMcjWE37xYr4U6rcATH%2Fimage.png?alt=media&token=03ca9e29-650e-49df-b338-7bed82b047a9) this format look like encode ? it's seems like a hexascii. Now I grab it and send it to the Burp Suite decoder.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMdG7CpC96NiteANxO-%2Fimage.png?alt=media&token=6ca26ec4-e71e-4075-9f3d-d736a77bd630)

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMcjeX5k_k9RcD_Jgoe%2Fimage.png?alt=media&token=d5179306-ddc9-47d4-8f2a-dd7cc6849db8)

That's right this is a hexascii. and this command it's run by mrb3n and this account can be use sudo permission.

## [](#header-2)Found another account in audit.log file.

Now we know the format. I used regex to search hexascii format, After I spend for a while I found something interesting!.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMd1LSySI79ghS2zFtv%2Fimage.png?alt=media&token=e3f14254-844b-4c14-a06a-28ef15b03ae3)

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMdEcl6JrJtbVTQVUob%2Fimage.png?alt=media&token=00b15b6d-ab01-472a-b677-a9e36818f173)


Next step I ues this credential to login.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMdFBAdY0prAbMxScrb%2Fimage.png?alt=media&token=71cd4662-2143-4855-a4be-2d198cd7de5b)

And I found some useful information in <b>audit.log</b>

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMclysFiox32mqNPbJP%2Fimage.png?alt=media&token=27b95514-7544-4db5-bf34-77ce2b8b2043)

As mentioned above this account can run with root through composer

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMdN9q73T24BKWE2U_Q%2F-MMdX-rWVkoABzFAXBuh%2Fimage.png?alt=media&token=98e02523-b1db-4004-aafb-811e8d836cff)

Now I know we can privilege escalation from composer.

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMdN9q73T24BKWE2U_Q%2F-MMdXdJvwYraXb6Ie3om%2Fimage.png?alt=media&token=605e041e-8323-4e17-b43c-92a06937de84)

## [](#header-2) Got root.txt

![](https://gblobscdn.gitbook.com/assets%2F-MBTznZrC2SIwzD_NiXo%2F-MMcdphVIZZmD7kJFFXT%2F-MMdN6Rh5NKbyfrD4_YT%2Fimage.png?alt=media&token=8e821ae7-041e-4395-9f42-008cbdc3efa1)

# [](headder-1)Ref:

[How can I read the audit time stamp? auditlog](https://www.linuxquestions.org/questions/linux-software-2/how-can-i-read-the-audit-time-stamp-msg%3Daudit-1213186256-105-20663-a-648547/)

[UNDERSTANDING AUDIT LOG FILES](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-understanding_audit_log_files)   

[Composer privilege escalation](https://gtfobins.github.io/gtfobins/composer/)

[Regular expression for a hexadecimal number](https://stackoverflow.com/questions/9221362/regular-expression-for-a-hexadecimal-number)