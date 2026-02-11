# 🚨 Intrusion Detection Lab (Snort + Ubuntu + Metasploitable + Kali)

![Snort](https://img.shields.io/badge/IDS-Snort-red)
![Ubuntu](https://img.shields.io/badge/OS-Ubuntu-orange)
![Kali](https://img.shields.io/badge/Attacker-Kali%20Linux-blue)
![Metasploitable](https://img.shields.io/badge/Target-Metasploitable2-green)
![Wireshark](https://img.shields.io/badge/Tool-Wireshark-lightgrey)


## 📌 Overview
This is a **small IDS lab project** where I used **Snort** on Ubuntu to detect malicious traffic (ICMP pings and Nmap scans) coming from Kali Linux against a Metasploitable VM.  

The goal of this project is to **practice intrusion detection** in a home lab setup and demonstrate how custom Snort rules can catch simple attacks.  

👉 **Read the full project walkthrough with screenshots here:**  
[🔗 Full Blog Post](https://secbyarafai.blogspot.com/2025/10/project-2-snort-sentinel-practical-ids.html)

---

## 🖥️ Lab Topology
- **Ubuntu (Snort IDS)** → runs Snort and stores alerts/logs.
- **Kali Linux (Attacker)** → performs pings and Nmap scans  
- **Metasploitable 2 (Target)** → vulnerable machine  

> All machines are connected to the same **internal/host-only network**: `192.168.113.0/24`

---

## 🛠️ Tools Used
- Ubuntu (Snort IDS)  
- Kali Linux (attacker)  
- Metasploitable 2 (target)  
- Wireshark (traffic analysis)
- Nmap (scanning tool)   

---
## 🔑 Key Takeaways
- **Snort reliably detects simple network probes** (ICMP pings, SYN scans) when rules are correctly written and HOME_NET is set.  
- **Rule syntax is strict** — small typos (missing spaces, wrong separators) will break parsing; always test with `sudo snort -c /etc/snort/snort.conf -T`.  
- **Immediate alerts are noisy** — removing thresholds shows activity instantly (useful for demos), but thresholds/detection_filter are necessary for realistic deployments to reduce false positives.  
- **Isolated lab networks are essential** — keep Metasploitable and scanning traffic on a host-only/internal network to avoid accidental exposure.  
- **Next steps:** integrate alerts into a SIEM (ELK/SecurityOnion), expand rule coverage, and tune thresholds for production-like behaviour.


## ⚙️ Setup Guide

### 1️⃣ Install Snort on Ubuntu
```bash
sudo apt update
sudo apt install snort -y
```
During installation, I set the HOME_NET variable to my subnet  192.168.113.0/24.
```bash
var HOME_NET 192.168.113.0/24
```
### Edit Config
```bash
sudo nano /etc/snort/snort.conf
```
###2️⃣ Add Custom Snort Rules

Open the local rules file:
```bash
sudo nano /etc/snort/rules/local.rules
```
Add the following rules:
```bash
# Detect ICMP pings
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:100001; rev:1;)

# Detect Nmap SYN scans
alert tcp any any -> $HOME_NET any (msg:"NMAP SYN scan detected"; flags:S; sid:1000001; rev:1;)
```
Save and exit.

###3️⃣ Test Snort
```bash
sudo snort -T -c /etc/snort/snort.conf
```

###3️⃣ Run Snort

Run Snort in console alert mode on your interface (ens33 is an example — replace with your actual one):
```bash
sudo snort -A console -q -c /etc/snort/snort.conf -i ens33
```
###4️⃣ Testing the Rules

From Kali Linux:

Ping test (ICMP):
```bash
ping -c 3 <ip addr>
```
➡️ Snort should alert with: ICMP Ping Detected

Nmap SYN scan:

```bash
sudo nmap -sS -sV -A <metasploitable2 ip>
```
➡️ Snort should alert with: NMAP SYN scan detected


