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
  Then you can start the database server and metasploit
  
  ```
    ┌──(root㉿INE)-[~]
    └─# service postgresql start
    Starting PostgreSQL 16 database server: main.
    
    ┌──(root㉿INE)-[~]
    └─# msfconsole 
  ```
  For a more organized setup, it's recommended to add a workspace in msf, in this case is going     to be called enum_ctf 
  
  ```
  msf6 > workspace -a enum_ctf                                                                      
  [*] Added workspace: enum_ctf
  [*] Workspace: enum_ctf
  ```
  We can begin with checking the smb version that's running. Also, set the global avariables with the `setg RHOST target.ine.local` command

  <img width="1905" height="621" alt="Screenshot 2025-09-11 at 10 36 50 PM" src="https://github.com/user-attachments/assets/7514a170-35fc-47ec-8418-9a9de5b2cc79" />

  
  In order to look for the auxiliary module of smb that's going to be used, try the following   command ` search type:auxiliary name:smb`  Since the first flag states that there's a share, let's use the `auxiliary/scanner/snmp/snmp_enumshares` module to enumerate the shares.
  <img width="1173" height="274" alt="Screenshot 2025-09-11 at 10 28 26 PM" src="https://github.com/user-attachments/assets/bfd8f14e-6bcd-459b-b1e0-3b9a09ae1521" />
  
  Then you can run it, and the system will find two shares: `print$` and `IPC$`
  <img width="1894" height="187" alt="Screenshot 2025-09-13 at 1 59 11 PM" src="https://github.com/user-attachments/assets/a5aa0962-1286-46be-a90c-2f6bc036d4f1" />

  Now, let's bruteforce the shares since none of them are accesible, there's a wordlists available on the desktop folder: /root/Desktop/wordlists/shares.txt
  <img width="1920" height="973" alt="Screenshot 2025-09-13 at 2 09 53 PM" src="https://github.com/user-attachments/assets/88b7b481-26a0-46d2-ad92-8fde4db49866" />
  You can create a bash file to do it. In my case I named it `enumeration.sh` also make sure to change the permissions of the file so it can be executable. 
  
  ```
  ┌──(root㉿INE)-[~]
  └─# chmod +x enumeration.sh 
  ```
  After running the file, we found htat there'sa share accessible called `pubfiles` 
  <img width="1915" height="937" alt="Screenshot 2025-09-13 at 2 24 05 PM" src="https://github.com/user-attachments/assets/1fb0523d-fd67-49b2-b8d6-77217ffb80e7" />

  Now we have to retreive the list of users, for that use the auxiliary module `scanner/smb/smb_enumusers`
  <img width="1358" height="591" alt="Screenshot 2025-09-13 at 2 44 58 PM" src="https://github.com/user-attachments/assets/1ce60cde-5810-4178-abf1-93ec5ac12c52" />
  
  As a result we have Josh, Nancy, Bob. Now, we can try to get the password. Let's work with the auxiliary module: `scanner/smb/smb_login`
  Since we have three users, let's set the variable USER_FILE with a document we are going to create and going to call `user.txt`

  ```
  msf6 auxiliary(scanner/smb/smb_login) > echo "josh,nancy,bob" > user.txt
  [*] exec: echo "josh,nancy,bob" > user.txt
  ```
   <img width="1733" height="797" alt="Screenshot 2025-09-13 at 2 53 29 PM" src="https://github.com/user-attachments/assets/15f3b0d0-3dad-46e4-9210-797e7910b641" />



  

  
  
  
  
  
