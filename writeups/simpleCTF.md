# 🔐 Simple CTF Writeup

## Target Information
- Target IP: 10.48.131.34
- Platform: TryHackMe (Simple CTF)

---

## 🔍 Enumeration

### Network Scan

nmap -A -sS -Pn -sV 10.48.131.34

### Results:

- 21/tcp → FTP  
- 80/tcp → HTTP  
- 2222/tcp → SSH  

### Analysis:
- Web server is the main entry point  
- SSH runs on a non-standard port  

---

## 🌐 Web Enumeration

### Directory Brute Force:

gobuster dir -u http://10.48.131.34 -w <wordlist>

### Findings:
- /simple directory discovered  
- Identified CMS Made Simple  

### Observation:
- Version not directly visible  
- Identified via vulnerability research  

---

## 💥 Exploitation

### Actions:
- Searched for CMS Made Simple vulnerabilities  
- Found public exploit  
- Executed exploit  

### Result:
- Retrieved valid credentials  

---

## 🔐 Initial Access

### SSH Login:

ssh <user>@10.48.131.34 -p 2222

### Result:
- User access gained  
- Found user.txt  

---

## 🔐 Privilege Escalation

### Check sudo permissions:

sudo -l

### Finding:
- Allowed to run vim without password  

### Exploit:

sudo vim -c ':!/bin/bash'

### Result:
- Root shell obtained  

---

## 📂 Post Exploitation

### Findings:
- User flag → Retrieved  
- Root flag → Retrieved  

---

## 🧠 Key Takeaways

- Outdated CMS leads to exploitation  
- Credential exposure is critical  
- Misconfigured sudo permissions are dangerous  
- Always check sudo -l  

---

## 🏁 Final Result

- User access: ✅ Achieved  
- Root access: ✅ Achieved  
- Flags captured: ✅ Complete  