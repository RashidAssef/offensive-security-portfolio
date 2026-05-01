# 🥒 Pickle Rick Writeup

## Target Information
- Target IP: 10.48.145.236
- Platform: TryHackMe (Pickle Rick)

---

## 🔍 Enumeration

### Network Scan

Performed initial scan to identify open ports and services:

nmap -Pn -A 10.48.145.236

### Results:

- 22/tcp → SSH  
- 80/tcp → HTTP  

### Analysis:
- Web server is the primary attack surface  
- SSH not accessible due to authentication restrictions  

---

## 🌐 Web Enumeration

### Findings:
- Website appeared minimal
- Inspected page source → Found username

### Directory Brute Force:

gobuster dir -u http://10.48.145.236 -w <wordlist> -t 50 -x php,html,txt

### Discovered:
- /login.php
- /robots.txt

### Key Insight:
- robots.txt contained a password-like string

---

## 🔐 Authentication

Used discovered credentials:

- Username → From HTML source  
- Password → From robots.txt  

### Result:
- Successfully logged into the application
- Gained access to a command execution panel

---

## 💥 Exploitation

### Observations:
- Some commands blocked (cat, touch)
- Allowed commands: ls, ls -la

### Actions:
- Enumerated directories
- Accessed files indirectly via URL

### Result:
- Retrieved 1st ingredient

---

## 🔁 Reverse Shell

### Setup listener:
nc -lvnp 9999

### Action:
- Used Python reverse shell

### Result:
- Gained interactive shell access

---

## 🔐 Privilege Escalation

sudo su

### Result:
- No password required
- Immediate root access

---

## 📂 Post Exploitation

### Findings:
- /home/rick/ → 2nd ingredient  
- /root/ → 3rd ingredient  

---

## 🧠 Key Takeaways

- Sensitive data exposure (HTML + robots.txt)
- Weak command filtering leads to bypass
- Reverse shells are essential for full control
- Misconfigured sudo = instant root

---

## 🏁 Final Result

- Shell access: ✅ Achieved  
- Root access: ✅ Achieved  
- All ingredients: ✅ Collected  