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

  now to access that share run this command: `smbclient \\\\target.ine.local\\pubfiles` once inside there's the flag text, retreive it with the get command
  <img width="886" height="257" alt="Screenshot 2025-09-13 at 3 05 43 PM" src="https://github.com/user-attachments/assets/f2992d78-80a3-43bc-9c3d-5c593c12b8ef" />

  The `flag1.txt` file is now available on the desktop. There's the first flag. 
  
  <img width="377" height="68" alt="Screenshot 2025-09-13 at 3 08 21 PM" src="https://github.com/user-attachments/assets/3050020c-24d3-4537-a345-eb57d0f27d82" />

  Second flag states the following: One of the samba users have a bad password. Their private share with the same name as their username is at risk!

   Now we have to retreive the list of users, for that use the auxiliary module `scanner/smb/smb_enumusers`  As a result we have Josh, Nancy, Bob.
  <img width="1358" height="591" alt="Screenshot 2025-09-13 at 2 44 58 PM" src="https://github.com/user-attachments/assets/1ce60cde-5810-4178-abf1-93ec5ac12c52" />
  
   Now, we can try to get the password. Let's work with the auxiliary module: `scanner/smb/smb_login` Set the SMB user as josh, nancy or bob and the PASS_FILE input `/usr/share/metasploit-  framework/data/wordlists/unix_passwords.txt`
  I took as first example josh, run the module and got the following output. password is purple
  <img width="978" height="670" alt="Screenshot 2025-09-13 at 3 20 40 PM" src="https://github.com/user-attachments/assets/cedeb3c4-c38c-4692-b641-8f58c95e428a" />

  For Nancy and Bob we didn't find any credentials.

    ```
    [*] target.ine.local:445  - Scanned 1 of 1 hosts (100% complete)
    [*] target.ine.local:445  - Bruteforce completed, 0 credentials were successful.
    [*] target.ine.local:445  - You can open an SMB session with these credentials and CreateSession set to true
    [*] Auxiliary module execution completed
    ```

  Now, let's try to access the SMB with josh's credentials
  <img width="689" height="242" alt="Screenshot 2025-09-13 at 3 26 32 PM" src="https://github.com/user-attachments/assets/5b44927f-1b4b-416f-8f79-80a973b63e75" />

  There's is flag 2
  <img width="685" height="104" alt="Screenshot 2025-09-13 at 3 27 51 PM" src="https://github.com/user-attachments/assets/977f8472-5747-4731-9ce1-66a6ad4bd4b0" />

  Flag 3 says: Follow the hint given in the previous flag to uncover this one. From it we know that there's a FTP service running. We can validate it with NMAP scan
   <img width="780" height="269" alt="Screenshot 2025-09-14 at 11 53 54 AM" src="https://github.com/user-attachments/assets/8cd5d113-ea55-412f-9132-1cfb2a425da7" />

   As a result we cn see that port 5554 is running with ftp service. 

   Let's check the version of the FTP with the auxiliary module ftp_version. Also, change the option of RPORT to 5554 as the service is running in that port. 
   <img width="1550" height="393" alt="Screenshot 2025-09-14 at 11 58 20 AM" src="https://github.com/user-attachments/assets/ebaefe0d-8fd3-489f-b50c-335893cc239e" />

   from there we got a banner: `FTP Banner: '220 Welcome to blah FTP service. Reminder to users, specifically ashley, alice and amanda to change their weak passwords immediately!!!\x0d\x0a'`
   Let's try to bruteforce it with the auxiliary module `auxiliary/scanner/ftp/ftp_login` change the options like the port and the username and userpass_file. In my case I'll try to run the password file against each user separately 
  <img width="1348" height="613" alt="Screenshot 2025-09-14 at 12 03 46 PM" src="https://github.com/user-attachments/assets/c00b03ba-744a-4d8a-833c-d29fc14d99cb" />


   

  
  
  
  
