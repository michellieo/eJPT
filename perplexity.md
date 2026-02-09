# T1046: Network Service Scanning

## Overview
- Category: Network / Metasploit
- Difficulty: Easy–Medium
- Format: Guided lab / CTF-style
- Artifacts: Lab VPN / target VMs

---

## Objective

Identify the ports open on the second target machine using appropriate Metasploit modules.

---

## Scenario

The lab environment and objective are described in the platform’s “Tasks” tab.

<img width="1918" height="999" alt="Screenshot 2025-08-22 at 6 38 45 PM" src="https://github.com/user-attachments/assets/fe7f9827-b329-4778-bb53-b04b71a1fed6" />

You start with network access to `demo1.ine.local` and must pivot to discover open ports on a second internal host.

---

## Skills Learned

- Enumeration
- Basic pivoting and routing within Metasploit
- Using Metasploit auxiliary scanners

---

## Tools Used

- Metasploit
- Bash
- Terminal
- Nmap

---

## Environment / Artifacts

- Initial target: `demo1.ine.local` (192.70.151.3)
- Attacker: Kali / INE attack box
- Second target: Host in 192.28.138.0/24 (discovered during the exercise)

---

## Flags to Capture

- Flag 1: IP address of the second target.
- Flag 2: Open TCP ports on the second target.
- Flag 3: Services running on those open ports.

(Adjust or rename flags as needed for your platform.)

---

## Hints (Optional)

- Always verify connectivity to the first target with `ping` or `nmap`.
- Use `sysinfo`, `ifconfig`, and routing features in Meterpreter to pivot.
- Metasploit’s `auxiliary/scanner/portscan/tcp` module can enumerate ports once routing is in place.

---

## Steps

1. Verify connectivity to the first target.

   ```bash
   ping demo1.ine.local
