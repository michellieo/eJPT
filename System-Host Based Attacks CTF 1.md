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
This means that WebDav directory does exits
Since the first question indicates that the user bob hasn't choose a strong password, we will perform an enumeration with Hydra 
<img width="1724" height="167" alt="Screenshot 2026-01-19 at 11 52 53 PM" src="https://github.com/user-attachments/assets/2527e6a9-2440-4ed0-8335-8bfc35d43246" />
As a result the password is: `password_123321`

