# 1. Host & Network Penetration Testing: System-Host Based Attacks CTF 1


### Objective
 Perform system/host-based attacks on the target and capture all the flags hidden within the environment

### Skills Learned
- Perform system/host-based attacks on Windows targets and identify hidden information on a target machine

### Tools Used

- Nmap
- Hydra
- Cadaver
- Metasploit Framework

### Flags to Capture:
- Flag 1: User 'bob' might not have chosen a strong password. Try common passwords to gain access to the server where the flag is located. (target1.ine.local)
- Flag 2: Valuable files are often on the C: drive. Explore it thoroughly. (target1.ine.local)
- Flag 3: By attempting to guess SMB user credentials, you may uncover important information that could lead you to the next flag. (target2.ine.local)
- Flag 4: The Desktop directory might have what you're looking for. Enumerate its contents. (target2.ine.local)

### Steps
First run an nmap with service detection -sV and script scan -sC
<img width="934" height="677" alt="Screenshot 2026-01-19 at 11 47 58 PM" src="https://github.com/user-attachments/assets/3b6b888f-6058-405a-b74b-15aeb8306b97" />

there's a service Microsoft IIS running on port 80, we can get more information about the port with nmap and a script 
<img width="831" height="217" alt="Screenshot 2026-01-19 at 11 49 49 PM" src="https://github.com/user-attachments/assets/9d9ec534-895a-461a-b945-1583cca2b296" />

This means that WebDav directory does exits.

Since the first question indicates that the user bob hasn't choose a strong password, we will perform an enumeration with Hydra 
<img width="1724" height="167" alt="Screenshot 2026-01-19 at 11 52 53 PM" src="https://github.com/user-attachments/assets/2527e6a9-2440-4ed0-8335-8bfc35d43246" />

As a result the password is: `password_123321`
Since we have access, we can check the directory /webdav 

<img width="1916" height="427" alt="Screenshot 2026-01-20 at 12 04 20 AM" src="https://github.com/user-attachments/assets/af33cef4-f0ec-4cca-aee4-2015dfe3365b" />

There's the first flag. 


For the second flag, we will upload a malicious .asp payload in order to obtain reverse shell

Now, let's run davtest to get a session (payload)

<img width="640" height="427" alt="Screenshot 2026-01-20 at 12 18 43 AM" src="https://github.com/user-attachments/assets/b3478207-b3e9-4a5b-b0ab-3ef6e1ab79af" />

After this, we will use cadaver utility that will allow us to go into webdav server and upload the webshell

<img width="648" height="175" alt="Screenshot 2026-01-20 at 12 20 32 AM" src="https://github.com/user-attachments/assets/e7e46637-0490-46f5-bd72-e812fea5ea14" />

Now, verify the webshell.asp file is on the browser GUI 

<img width="1914" height="356" alt="Screenshot 2026-01-20 at 12 21 37 AM" src="https://github.com/user-attachments/assets/8d3dfad4-4214-484a-82eb-af28086162e7" />

Double click the webshel.asp file and you'll see an index box that has access to root and can execute commands

<img width="1917" height="450" alt="Screenshot 2026-01-20 at 12 22 56 AM" src="https://github.com/user-attachments/assets/3714fcb3-f04b-47e4-9003-6550f7a3a6b7" />

Since we need to go to the C: direcotry, type `dir C:\`

<img width="1911" height="810" alt="Screenshot 2026-01-20 at 12 23 49 AM" src="https://github.com/user-attachments/assets/6c4b7569-e460-4f24-910b-69b34da51563" />

And the flag2.txt is visible, then `type C:\flag2.txt`
and you'll obtain the flag 2

For the 3rd flag, we get the propmt: SMB shares might contain hidden files. Check the available shares. (target2.ine.local). As this is a different host, we will perform a nmap scan first to detect the running services 

<img width="1808" height="846" alt="Screenshot 2026-01-26 at 10 46 22 PM" src="https://github.com/user-attachments/assets/6088eee4-4907-4559-9b75-96153c83cf95" />

There's is something runnng on port 445, SMB uses port 445. Alo we can seethe message: signing enabled but not required that means can be accesible through PsExec. Since PsExec can be automated through MSF, we will iniitate the service and look for the following module: smb_login 

```
┌──(root㉿INE)-[~]
└─# service postgresql start && msfconsole
```
after loking the module, we will look for the options of the module and configure them and run the module

<img width="1840" height="935" alt="Screenshot 2026-01-26 at 10 55 04 PM" src="https://github.com/user-attachments/assets/518043cf-ab2d-48a7-b96b-103eaa0b1fb6" />

```
msf6 auxiliary(scanner/smb/smb_login) > set RHOSTS target2.ine.local
RHOSTS => target2.ine.local
msf6 auxiliary(scanner/smb/smb_login) > set USER_FILE /usr/share/metasploit-framework/data/wordlists/common_users.txt
USER_FILE => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf6 auxiliary(scanner/smb/smb_login) > set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
PASS_FILE => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
```
As a result, we obtained 4 credentials for different users

<img width="1140" height="210" alt="Screenshot 2026-01-26 at 11 08 18 PM" src="https://github.com/user-attachments/assets/1276fe45-74d0-4e60-aa32-ba97e9692408" />

Let's use the MSF module for the exploit ow that we get the credentials: `exploit/windows/smb/psexec` 

<img width="1660" height="371" alt="Screenshot 2026-01-26 at 11 12 00 PM" src="https://github.com/user-attachments/assets/64d74d37-41b2-428f-bb5e-58f31ac3f9d0" />

<img width="784" height="138" alt="Screenshot 2026-01-26 at 11 13 54 PM" src="https://github.com/user-attachments/assets/adc90f9e-b1f2-457a-a971-70415f567208" />




