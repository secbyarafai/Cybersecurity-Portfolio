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
```
During installation, I set the HOME_NET variable to my subnet  192.168.113.0/24.
```bash
var HOME_NET 192.168.113.0/24
```
###2️⃣ Add Custom Snort Rules

Open the local rules file:
```bash
sudo vim /etc/snort/rules/local.rules
```
Add the following rules:
```bash
# Detect ICMP pings
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:100001; rev:1;)

# Detect Nmap SYN scans
alert tcp any any -> $HOME_NET any (msg:"NMAP SYN scan detected"; flags:S; sid:1000001; rev:1;)
```
Save and exit.
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
sudo nmap -sS -sV -A 192.168.113.133
```
➡️ Snort should alert with: NMAP SYN scan detected


