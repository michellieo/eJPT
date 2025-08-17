# 1. Assessment Methodologies: Footprinting and Scanning CTF 1


### Objective
Document the solution to the second assesment of eJPT exam.

### Skills Learned
- Network Fundamentals
- Host discovery with Nmap
- Port scanning with Nmap

### Tools Used

- NMAP

### Steps

Introduction page is the following:
<img width="1920" height="1001" alt="Screenshot 2025-08-15 at 11 49 23 PM" src="https://github.com/user-attachments/assets/5d0f10e0-b88c-43c3-92fc-c45be8e3a55e" />

when lab page gets opened you'll see a kali linux environmemt. According to the isntructions, the target will be found at: http://target.ine.local. 

- Flag 1: The server proudly announces its identity in every response. Look closely; you might find something unusual.

  First open terminal, then run the following command: nmap  -sC -sV  target.ine.local . As an output you'll have a detailed information per ports. And checking the output, you'll find the flag.
  
  <img width="1916" height="964" alt="Screenshot 2025-08-16 at 12 25 38 AM" src="https://github.com/user-attachments/assets/1664d84c-8277-4afe-9959-5ab384c76525" />

- Flag 2: The gatekeeper's instructions often reveal what should remain unseen. Don't forget to read between the lines.
  After analyzing the nmap output, you'll see that robot.txt has 3 entries that are disabled, one of them is called secret-info
  ```
  | http-robots.txt: 3 disallowed entries 
  |_/photos /secret-info/ /data/
  ```
  Go to the website and acces the directory of secret-info, you'll see a flag.txt document inside 
  <img width="1915" height="994" alt="Screenshot 2025-08-17 at 12 27 40 PM" src="https://github.com/user-attachments/assets/ae532868-f034-47ae-b130-04966f1cc2bc" />
  Then go to the directory including the flag.txt, and you'll find the flag
  
  <img width="1913" height="959" alt="Screenshot 2025-08-17 at 12 29 21 PM" src="https://github.com/user-attachments/assets/42e7c6a0-f5ae-4e57-a915-af1a98deae5d" />

  
- Flag 3: Anonymous access sometimes leads to forgotten treasures. Connect and explore the directory; you might stumble upon something valuable.
  Check again the nmap output, you'll see that the ftp service is open and can be accessed anonymously. Due to "Anonymous FTP login allowed"

  ```
  PORT     STATE SERVICE  VERSION
  21/tcp   open  ftp      vsftpd 3.0.5
  | ftp-anon: Anonymous FTP login allowed (FTP code 230)
  | -rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
  |_-rw-r--r--    1 0        0              39 Aug 17 16:53 flag.txt
  | ftp-syst: 
  |   STAT: 
  | FTP server status:
  |      Connected to ::ffff:192.85.108.2
  |      Logged in as ftp
  |      TYPE: ASCII
  |      No session bandwidth limit
  |      Session timeout in seconds is 300
  |      Control connection is plain text
  |      Data connections will be plain text
  |      At session startup, client count was 2
  |      vsFTPd 3.0.5 - secure, fast, stable
  |_End of status
  ```
  now to access the ftp server, user anonymours as username and hit enter in password
  ```
  ftp target.ine.local
  Connected to target.ine.local.
  220 (vsFTPd 3.0.5)
  Name (target.ine.local:root): anonymous
  331 Please specify the password.
  Password: 
  230 Login successful.
  Remote system type is UNIX.
  Using binary mode to transfer files.

  ```
  In order to chek what's stored inside the server, run the following command
  
  ```
  ftp> ls
  ```
  As a result from this command it will appear two .txt documents that match with the namp output

  ```
  229 Entering Extended Passive Mode (|||6715|)
  150 Here comes the directory listing.
  -rw-r--r--    1 0        0              22 Oct 28  2024 creds.txt
  -rw-r--r--    1 0        0              39 Aug 17 16:53 flag.txt
  ```
  now to get those files, we run the get command and transfer to the local machine
  
  <img width="1900" height="251" alt="Screenshot 2025-08-17 at 12 55 56 PM" src="https://github.com/user-attachments/assets/e3d755a8-b504-47e5-ae56-dd82c222756f" />

  Now we can visualize the content of the documents:

  ```
  cat flag.txt                                                                                                                                                                                                                           
  FLAG3_babd7c75168b49a291b3b32567b53e50
   cat creds.txt                                                                                                                                                                                                                          
  db_admin:password@123
  ```
  The flag is there, and we have the credentials for what it lookslike a database

- Flag 4: A well-named database can be quite revealing. Peek at the configurations to discover the hidden treasure.

  refer to the nmap output, you'll see that at the port 3306 there's a mysql service running
  to access the database run this command ``` mysql -h target.ine.local -u db_admin -p ``` login into it using the crednetials found on creds.txt
  <img width="681" height="215" alt="Screenshot 2025-08-17 at 1 12 18 PM" src="https://github.com/user-attachments/assets/d862e917-7be3-4a5b-98fc-b3e484a79a08" />

  since we have access, let's check the database

  ```
  MySQL [(none)]> show databases;
  +----------------------------------------+
  | Database                               |
  +----------------------------------------+
  | FLAG4_46a590b085d34b46bfd54bbe9f82f186 |
  | information_schema                     |
  | mysql                                  |
  | performance_schema                     |
  | sys                                    |
  +----------------------------------------+
  5 rows in set (0.001 sec)
  ```

