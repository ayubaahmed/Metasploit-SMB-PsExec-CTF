# Metasploit SMB PsExec CTF Lab (Walkthrough + Answers)

This project documents a full end-to-end workflow from a Capture The Flag (CTF) lab using the Metasploit Framework to simulate an SMB compromise and conduct post-exploitation with Meterpreter.  
It explains exactly how each answer was derived, with commands, reasoning, and evidence.

> Legal & Ethics  
> Activities shown here were performed in a controlled lab with explicit authorisation. Do not use these techniques on systems you do not own or have permission to test.

---

## Environment

- Attacker: Kali Linux (Metasploit Framework)  
- Target: Windows (domain-joined)  
- Access Vector: SMB (PsExec)  
- Credentials used: `ballen:Password1`  
- Payload: `windows/meterpreter/reverse_tcp`

---

## Initial Access (Summary)

- Used the `exploit/windows/smb/psexec` module.  
- Set `RHOST` to the target IP
- Set `SMBUser` to `ballen` 
- Set `SMBPass` to `Password1`
- Set payload to `windows/meterpreter/reverse_tcp`.  
- Ran the exploit to obtain a Meterpreter session.

After the session opened:

- Ran `ps` to list running processes.  
- Migrated to the `lsass.exe` process using `migrate <PID>` to gain SYSTEM privileges.


---

# Questions and How I Got Each Answer

Each section includes:

* The Reasoning (why that method answers the question)  
* The Steps & Commands (what I ran)  
* The Answer (final value)  
* The Evidence 

---

## 1) What is the computer name?

Reasoning:  
The computer name can be retrieved via Meterpreter's system info after obtaining a reverse shell.

Steps & Commands:

- Used `exploit/windows/smb/psexec` module.  
- Set `RHOST`, `SMBUser`, `SMBPass` and payload to `windows/meterpreter/reverse_tcp`.  
- Ran the exploit to get a Meterpreter session.  
- Ran `sysinfo` in the Meterpreter session to get the computer name.

Answer: `ACME-TEST`

[Compromisation Phase](screenshots/Compromisation-Phase.png)

---

## 2) What is the target domain?

Reasoning:  
Domain information can be enumerated using the `post/windows/gather/enum_domain` module.

Steps & Commands:

- Backgrounded the first Meterpreter session.  
- Used `post/windows/gather/enum_domain` module.  
- Set the `SESSION` parameter to the first session.  
- Ran the module to retrieve domain information.

Answer: `FLASH`

[Processes](./screenshots/Processes.png)

---

## 3) What is the name of the share likely created by the user?

Reasoning:  
User-created shares can be identified using the `post/windows/gather/enum_shares` module.

Steps & Commands:

- Used the `post/windows/gather/enum_shares` module.  
- Set the `SESSION` parameter to the first session.  
- Ran the module to list all shares.  
- Identified the non-default share likely created by the user.

Answer: `speedster`

Evidence to capture: screenshot of the shares list.

---

## 4) What is the NTLM hash of the `jchambers` user?

Reasoning:  
Local SAM hashes can be dumped by migrating to `lsass.exe` in a SYSTEM context.

Steps & Commands:

- Returned to the first Meterpreter session.  
- Ran `ps` to list all processes.  
- Migrated to the `lsass.exe` process using `migrate <PID>`.  
- Ran `hashdump` to retrieve NTLM hashes.  
- Extracted the line for `jchambers`.

Answer: `69596c7aa1e8daee17f8e78870e25a5c`

Evidence to capture: screenshot of the hashdump output.

---

## 5) What is the cleartext password of the `jchambers` user?

Reasoning:  
The NTLM hash can be cracked to reveal the cleartext password.

Steps & Commands:

- Copied the NTLM hash for `jchambers`.  
- Used CrackStation to crack the hash.  
- Obtained the cleartext password from the cracked hash.

Answer: `Trustno1`

Evidence to capture: redacted snippet showing cracked password.

---

## 6) Where is the `secrets.txt` file located? (Full path)

Reasoning:  
Meterpreter's `search` command can locate files on the target system.

Steps & Commands:

- Ran `search -f secrets.txt` in Meterpreter.  
- Navigated to the file path.  
- Verified file existence using `cat`.

Answer (full path): `c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt`

Evidence to capture: screenshot of search results and file contents.

---

## 7) What is the Twitter password revealed in the `secrets.txt` file?

Reasoning:  
The Twitter password is stored in the contents of `secrets.txt`.

Steps & Commands:

- Navigated to the `secrets.txt` file.  
- Ran `cat` on the file to view contents.  
- Extracted the Twitter password line.

Answer: `KDSvbsw3849!`

Evidence to capture: redacted snippet showing Twitter password.

---

## 8) Where is the `realsecret.txt` file located? (Full path)

Reasoning:  
The same search method can be used for `realsecret.txt`.

Steps & Commands:

- Ran `search -f realsecret.txt` in Meterpreter.  
- Navigated to the file path.  
- Verified file existence using `cat`.

Answer (full path): `c:\inetpub\wwwroot\realsecret.txt`

Evidence to capture: screenshot of search results.

---

## 9) What is the real secret?

Reasoning:  
The real secret is stored in `realsecret.txt`.

Steps & Commands:

- Navigated to `realsecret.txt`.  
- Ran `cat` to view the file contents.  
- Recorded the real secret.

Answer: `The Flash is the fastest man alive`

Evidence to capture: screenshot of file content (redact if necessary).

---

## Evidence Folder

Place supporting screenshots in `/screenshots` and reference them inline:

---

## Key Takeaways

- Practised authenticated SMB access using `psexec` and established a Meterpreter session.  
- Migrated into `lsass.exe` to achieve SYSTEM context for credential dumping.  
- Cracked NTLM hashes to obtain cleartext passwords.  
- Discovered and exfiltrated sensitive files to answer investigation questions.  
- Documented a clear, reproducible methodology for each question with evidence.
