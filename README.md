# SMB Reconnaissance & Authentication Investigation

## 🧾 Overview

This project simulates SMB reconnaissance and authentication attempts within a controlled lab environment. The objective was to detect attacker behavior and correlate endpoint logs with observed network activity.

---

## 🎯 Objectives

* Identify SMB reconnaissance activity
* Analyze authentication attempts using Windows Event Logs
* Correlate attacker actions with endpoint telemetry
* Map activity to MITRE ATT&CK techniques

---

## 🛠️ Tools Used

* Kali Linux (Attacker)
* Nmap
* smbclient
* Windows 10 (Target)
* Windows Event Viewer
* Sysmon (optional if used)
* Wazuh SIEM (if applicable)

---

## 🧪 Lab Setup

* Attacker: Kali Linux VM
* Target: Windows VM
* Network: VirtualBox internal network

---

## 🔍 Investigation

### 1. SMB Reconnaissance (Nmap Scan)

Attacker performed port scanning targeting SMB (port 445):

* Initial state: **filtered**
* Later state: **open**

This indicates potential changes in firewall behavior or service exposure.

---

### 2. SMB Enumeration Attempt

Command used:

```
smbclient -L //192.168.56.102 -N
```

Result:

```
NT_STATUS_ACCESS_DENIED
```

This indicates unauthorized access attempt against SMB shares.

---

### 3. Authentication Log Analysis (Windows)

Observed key security events:

* **Event ID 4625** → Failed logon attempts
* **Event ID 4624** → Successful logon
* **Event ID 4634** → Logoff activity
* **Event ID 5379** → Credential access (Credential Manager)

---

### 4. Log Correlation

* Multiple failed logons (4625) suggest brute-force behavior
* Successful logon (4624) following failures may indicate credential compromise
* Credential access (5379) suggests possible post-authentication activity

---

## 🚨 Findings

* SMB reconnaissance detected via Nmap scan
* Unauthorized SMB enumeration attempt identified
* Repeated failed authentication attempts observed
* Credential-related activity detected post-authentication

---

## 🧬 MITRE ATT&CK Mapping

* **T1046** – Network Service Discovery
* **T1078** – Valid Accounts
* **T1110** – Brute Force
* **T1555** – Credentials from Password Stores

---

## 📌 Indicators of Compromise (IOCs)

* Source IP: 192.168.56.X (attacker machine)
* Multiple failed logon attempts (Event ID 4625)
* SMB access attempts with denied authentication
* Credential Manager access event (5379)

---

## 📸 Screenshots

### Nmap Scan Results

(Add your first screenshot here)

### SMB Access Attempt

(Add smbclient screenshot here)

### Windows Event Logs (4625, 4624, 5379)

(Add your Event Viewer screenshot here)

---

## 📊 Outcome

Successfully identified and correlated SMB reconnaissance, authentication attempts, and credential access activity across attacker and endpoint logs.

---

## 🧠 Lessons Learned

* SMB (port 445) is a high-value target for attackers
* Failed logons (4625) are critical indicators of brute-force attempts
* Credential events (5379) can indicate post-exploitation behavior
* Correlation between network activity and logs is essential for detection
