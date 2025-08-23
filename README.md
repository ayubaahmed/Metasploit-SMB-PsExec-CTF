# Metasploit SMB PsExec CTF Lab Walkthrough

This repository contains a complete walkthrough of a Capture The Flag (CTF) lab using the Metasploit Framework to simulate an SMB compromise and perform post-exploitation with Meterpreter. The walkthrough includes step-by-step explanations, commands, and screenshots as evidence.

> **Legal & Ethics**  
> All activities in this lab were carried out in a controlled environment with full authorisation. These techniques should **not** be used on any systems you do not own or have explicit permission to test.

---

## Repository Structure


---

## Lab Overview

- **Attacker:** Kali Linux (Metasploit Framework)  
- **Target:** Windows (domain-joined)  
- **Access Vector:** SMB (PsExec)  
- **Credentials:** `ballen:Password1`  
- **Payload:** `windows/meterpreter/reverse_tcp`

The lab covers:

- Gaining an initial Meterpreter session via SMB  
- Privilege escalation to SYSTEM  
- Credential dumping and hash cracking  
- Searching for and extracting sensitive files  
- Documenting each step clearly with evidence

---

## How to Use This Repository

1. Open `report.md` to follow the full walkthrough. Each section includes:
   - Reasoning for each step  
   - Commands executed  
   - Answers obtained  
   - Screenshots showing evidence
2. Check the `/screenshots` folder for all supporting images.


## Key Takeaways

- Practised authenticated SMB access and established a Meterpreter session.  
- Migrated to `lsass.exe` to gain SYSTEM privileges for credential dumping.  
- Cracked NTLM hashes to recover cleartext passwords.  
- Located sensitive files and extracted their contents.  
- Maintained a clear, reproducible methodology, supported by screenshots for each step.
