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
  If you log into target.ine.local/robots.txt directory you'll see some disallowed links/directories
   <img width="1234" height="672" alt="Screenshot 2025-10-17 at 5 16 16 PM" src="https://github.com/user-attachments/assets/8fcaac6a-d337-445b-819d-b75d348d8b59" />
  We can log into target.ine.local/phpmyadmin
  
  The second flag was found in the database "mysql" in the secret_info table 
    <img width="1279" height="699" alt="Screenshot 2025-10-13 at 2 23 58 AM" src="https://github.com/user-attachments/assets/56fd43b6-cda6-4fca-9919-8b56bd26af53" />

  The clue for the thir flag is: A PHP file that displays server information might be worth examining. What could be hidden in plain sight?
    The information of the php server is stored in phpinfo.php file, if we look carefully we will find the thrid flag.
    <img width="1234" height="663" alt="Screenshot 2025-10-17 at 5 23 34 PM" src="https://github.com/user-attachments/assets/b9c84ee8-69c0-4a8a-8e4c-97652dcb165a" />

  For the latest flag, this is the informaation we got: Sensitive directories might hold critical information. Search through carefully for hidden gems.
    From the gobuster resut, we can see that there's directory called passwords, if you acces it, you'll see a file called flag.txt, there's the 4th flag
    <img width="1220" height="646" alt="Screenshot 2025-10-17 at 5 27 04 PM" src="https://github.com/user-attachments/assets/b5f392d9-8f31-4cb9-ad34-9b5d4f161bdc" />

That was all, we have completed the lab
<img width="1669" height="891" alt="Screenshot 2025-10-17 at 5 28 42 PM" src="https://github.com/user-attachments/assets/9323c701-66bb-4380-bb60-45d88586bed2" />

