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
