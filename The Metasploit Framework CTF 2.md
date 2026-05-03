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


