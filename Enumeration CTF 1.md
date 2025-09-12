# 1. Assessment Methodologies: Enumeration CTF 1


### Objective
  Document the solution to the third assesment of eJPT exam.

### Skills Learned
  - Network Enumeration

### Tools Used
  
  - NMAP
  - Metasploit

### Steps
  
  The main task page looks like this: 
  <img width="1920" height="968" alt="Screenshot 2025-09-11 at 10 11 07 PM" src="https://github.com/user-attachments/assets/ad9fc862-8562-45ab-a73f-b3d564fe536b" />

  The  machine is target.ine.local, the first flag says: There is a samba share that allows anonymous access. Wonder what's in there!
  Let's try the SMB enumeration. Make sure the machine is accesible with ping command
  
  ```
  ┌──(root㉿INE)-[~]
  └─# ping -c 2 target.ine.local
  PING target.ine.local (192.215.252.3) 56(84) bytes of data.
  64 bytes from target.ine.local (192.215.252.3): icmp_seq=1 ttl=64 time=0.097 ms
  64 bytes from target.ine.local (192.215.252.3): icmp_seq=2 ttl=64 time=0.086 ms
  
  --- target.ine.local ping statistics ---
  2 packets transmitted, 2 received, 0% packet loss, time 1002ms
  rtt min/avg/max/mdev = 0.086/0.091/0.097/0.005 ms

  ```
  
