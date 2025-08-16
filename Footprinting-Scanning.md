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

