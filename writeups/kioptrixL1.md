# Kioptrix Level 1 Writeup

## Target Information
- Target IP: 192.168.0.220
- Platform: VulnHub (Kioptrix Level 1)

---

## 🔍 Enumeration

### Network Scan

Performed an aggressive scan to identify open ports and services:

nmap -A 192.168.0.220

### Results:

- 22/tcp → SSH
- 80/tcp → HTTP
- 111/tcp → RPC
- 139/tcp → SMB
- 443/tcp → HTTPS
- 32768/tcp → Unknown service

### Analysis:
- Web services (80, 443) showed no useful information
- SMB (139) looked promising for further enumeration

---

## 📂 SMB Enumeration

Used Metasploit to identify SMB version.

### Finding:
- Samba version: 2.2.1a

This version is known to be vulnerable to remote exploits.

---

## 💥 Exploitation

Searched for available exploits:

searchsploit samba 2.2

Identified:
- trans2open exploit

### Steps:
1. Launched Metasploit
2. Selected exploit for Samba 2.2
3. Configured payload:

generic/shell_reverse_tcp

4. Executed exploit

### Result:
Successfully gained shell access to the system.

---

## 🔐 Privilege Escalation

Initial access provided high privileges.

### Actions:
- Accessed sensitive file:

cat /etc/shadow

- Generated new password:

openssl passwd -1 -salt <salt> <password>

- Created modified shadow file
- Replaced original shadow file

### Result:
Obtained persistent root access.

---

## 🧠 Key Takeaways

- Outdated services can lead to full system compromise
- SMB misconfigurations are high-risk
- Always enumerate service versions carefully
- Exploit databases are critical in real-world attacks

---

## 🏁 Final Result

- Root access: ✅ Achieved
- System fully compromised