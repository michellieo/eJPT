# 1. Assessment Methodologies: Vulnerability Assesment CTF 1


### Objective
Document the solution to the lab assesment of eJPT course.

### Skills Learned
- Passive and active information gathering

### Tools Used

- NMAP
- Nessus

### Steps

  The lab environment is that we will have a Nessus dashborad and a target machine with credentials
  <img width="1920" height="970" alt="Screenshot 2025-10-13 at 1 27 01 AM" src="https://github.com/user-attachments/assets/a022942d-b070-4bae-989c-06eee41392ef" />

  As a first flag, we got the following: Explore hidden directories for version control artifacts that might reveal valuable information.
  Since we are talking about a hidden directory, we are going to use the gobuster tool. running the following command ``` gobuster dir -u http://taget.ine.local -w /usr/share/wordlists/dirb/common.txt -t 30 -o results.txt```
  From the enumeration utput, we select the directories with the HTTP response with 200 and open them in the browser.
  <img width="1044" height="566" alt="Screenshot 2025-10-13 at 1 53 20 AM" src="https://github.com/user-attachments/assets/36bfd66d-ba34-4a00-9323-9923d9e09dce" />
  The .git drectory has a flag.txt document, there's our flag. 
  <img width="1233" height="671" alt="Screenshot 2025-10-13 at 1 55 19 AM" src="https://github.com/user-attachments/assets/c08bbb84-d22e-4533-82f6-9c46fede9efe" /> 

  Second Flag has this statement: The data storage has some loose security measures. Can you find the flag hidden within it?
  
  The second flag was found in the database "mysql" 
    <img width="1279" height="699" alt="Screenshot 2025-10-13 at 2 23 58 AM" src="https://github.com/user-attachments/assets/56fd43b6-cda6-4fca-9919-8b56bd26af53" />

  The clue for the thir flag is: A PHP file that displays server information might be worth examining. What could be hidden in plain sight?
