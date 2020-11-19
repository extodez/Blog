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

