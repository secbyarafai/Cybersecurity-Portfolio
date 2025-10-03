# 🚨 Intrusion Detection Lab (Snort + Ubuntu + Metasploitable + Kali)

## 📌 Overview
This project is a small **Intrusion Detection System (IDS) lab** designed for learning and portfolio demonstration.  
It uses **Snort** (running on Ubuntu) to detect malicious traffic such as ICMP pings and Nmap scans inside a controlled virtual environment.

---

## 🖥️ Lab Topology
- **Ubuntu (Snort IDS)** → 192.168.113.133 (example)  
- **Kali Linux (Attacker)** → performs pings and Nmap scans  
- **Metasploitable 2 (Target)** → vulnerable machine  

> All machines are connected to the same **internal/host-only network**: `192.168.113.0/24`

---

## 🛠️ Tools Used
- Ubuntu (Snort IDS)  
- Kali Linux (attacker)  
- Metasploitable 2 (target)  
- Wireshark / tcpdump (traffic analysis)  
- nmap (scanning tool)  

---

## ⚙️ Setup Guide

### 1️⃣ Install Snort on Ubuntu
```bash
sudo apt update
sudo apt install snort

uggughguj
