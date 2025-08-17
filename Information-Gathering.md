# 1. Assessment Methodologies: Information Gathering CTF 1


### Objective
Document the solution to the first assesment of eJPT exam.

### Skills Learned
- Passive and active information gathering

### Tools Used

- NMAP

### Steps

this is the page of instruction: 
<img width="1876" alt="Screenshot 2025-06-26 at 6 10 44 PM" src="https://github.com/user-attachments/assets/dd2cb22c-1187-466f-baaf-b333a16fe488" />

at first, when you open the lab you'll see a kali linux page like this one:
<img width="1918" alt="Screenshot 2025-06-26 at 6 23 01 PM" src="https://github.com/user-attachments/assets/4318d058-e57a-4d7f-993c-48a46243ff52" />


then after reading the first question that is: 
- Flag 1: This tells search engines what to and what not to avoid.

  we can check it with the file robots.txt. 

  let's open the browser and go to the website,  http://target.ine.local
  then go to the http://target.ine.local/robots.txt 
  <img width="1137" alt="Screenshot 2025-06-26 at 6 25 56 PM" src="https://github.com/user-attachments/assets/48a0225f-69ca-4a27-a196-f7e17a008c26" />
  then there's the flag

- Flag 2: What website is running on the target, and what is its version?
  First I tried to look for it with sitemap, but got an error: 
  ![Screenshot 2025-06-30 at 12 43 18 AM](https://github.com/user-attachments/assets/c09345f0-a96d-4ebd-989f-f50377b1fd85)

  That's why I tried with nmap

  to get the ip, I get the command host and address 
  ![Screenshot 2025-06-30 at 12 54 11 AM](https://github.com/user-attachments/assets/33f6b110-c668-4c92-9820-9f3ac84f559c)

  run the nmap command, and got the output: 
  ![Screenshot 2025-06-30 at 12 55 53 AM](https://github.com/user-attachments/assets/fa00eac6-8b39-4892-a55b-1d9da71aeff4)

  there's the flag
  ![Screenshot 2025-06-30 at 12 58 16 AM](https://github.com/user-attachments/assets/4cb9b470-2398-4c2f-97fe-c4da4130d8cd)

- Flag 3: Directory browsing might reveal where files are stored.
  For this one I used gobuster, and learned about it here: https://medium.com/@dcsenadheera777/directory-file-brute-forcing-and-enumeration-using-gobuster-8141a847f2f7

  opened the cmd and run the cmd console, and run the gobuster command:
  ![Screenshot 2025-06-30 at 1 12 42 AM](https://github.com/user-attachments/assets/e93f588d-a739-4d34-8447-2d61a82e35c9)
  then checked the pages that had a response with 200 or 301 
  opened the wp-content directory and saw a major list of directories as the image: 

  ![Screenshot 2025-06-30 at 1 17 00 AM](https://github.com/user-attachments/assets/28542f82-ef63-46d0-bdc7-e46be6dfb1ff)

  then run the command again in gobuster inclufing wp-includes and got this output: 
  ![Screenshot 2025-06-30 at 1 12 54 AM](https://github.com/user-attachments/assets/318142da-8554-466b-a145-c04715c9a6d3)

  checked the page again and saw a file with flag name on it, opened and saw the answer: 
  ![Screenshot 2025-06-30 at 1 13 30 AM](https://github.com/user-attachments/assets/a4ff8e19-73fc-48bb-890a-6649fbda6333)

- Flag 4: An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.

  After some research on google, I found that the file is "wp-config.bak" , and via cli got the information with the curl command. And there's the flag
  n![Screenshot 2025-06-30 at 1 45 05 AM](https://github.com/user-attachments/assets/612616d9-2d38-42b5-886f-8f80bf5530d2)
  ![Screenshot 2025-06-30 at 1 46 06 AM](https://github.com/user-attachments/assets/4e3b8dc6-384d-4cff-b259-a6bb79a73f5b)

- Flag 5: Certain files may reveal something interesting when mirrored.

  the httrack tool help us to mirror the website 

  ![Screenshot 2025-06-30 at 1 58 45 AM](https://github.com/user-attachments/assets/21b1985d-fab5-485b-aaa4-d23fa7177ab8)

  after checking the location of the mirrored website, I found a .php file, then I cat the file to see the content and founf the flag fot the last question.
  ![Screenshot 2025-06-30 at 1 59 04 AM](https://github.com/user-attachments/assets/6560d64a-12f4-4e14-90e3-c4b74c14ea10)

now, lab is completed! 


![Screenshot 2025-06-30 at 2 02 32 AM](https://github.com/user-attachments/assets/f7630e60-5129-4f8c-8191-99794b1e4026)



