
# Windows Firewall Configuration and Port Blocking

## Overview
Configured Windows Defender Firewall to block incoming Telnet traffic (TCP port 23) and tested connectivity using Kali Linux.

## Tools Used
- Windows PowerShell
- Windows Defender Firewall
- Kali Linux
- UFW (Uncomplicated Firewall)
- Netcat (NC)
- Oracle VirtualBox

## Tasks Performed
- Checked Windows Firewall profiles and services.
- Created an inbound firewall rule to block TCP port 23.
- Configured Kali UFW to allow SSH (22) and deny Telnet (23).
- Tested network connectivity before and after firewall configuration.
- Verified firewall rules and listening ports.

## Actual Test Results

### 1. Initial Telnet Connection Test

**Command:**
```bash
nc -vz -w 5 <WINDOWS_IP> 23
```

**Output:**
```text
Connection to <WINDOWS_IP> 23 (telnet) open
```

**Result:** Port 23 was initially accepting connections.

### 2. Windows Firewall Rule Verification

**Command:**
```powershell
Get-NetFirewallRule -DisplayName "Task4_Block_Telnet_23"
```

**Results:**
- Enabled: True
- Direction: Inbound
- Action: Block
- Protocol: TCP
- Local Port: 23
- Profile: Any

### 3. Windows Port Listening Check

**Command:**
```powershell
Get-NetTCPConnection -State Listen -LocalPort 23
```

**Initial Output:**
```text
LocalPort : 23
State     : Listen
```

**Later Output:**
```text
No matching MSFT_NetTCPConnection objects found
```

**Result:** Port 23 was no longer shown as listening.

### 4. Kali Linux UFW Configuration

**Commands:**
```bash
sudo ufw allow 22/tcp
sudo ufw deny 23/tcp
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
sudo ufw status verbose
```

**Actual Results:**
```text
Status: active

Default: deny (incoming), allow (outgoing)

22/tcp       ALLOW IN    Anywhere
23/tcp       DENY IN     Anywhere
22/tcp (v6)  ALLOW IN    Anywhere (v6)
23/tcp (v6)  DENY IN     Anywhere (v6)
```

### 5. Final Telnet Connection Test

**Command:**
```bash
nc -vz -w 5 <WINDOWS_IP> 23
```

**Output:**
```text
Connection refused
```

**Result:** The Telnet connection was rejected.

### 6. SSH Connectivity Test

**Command:**
```powershell
Test-NetConnection -ComputerName <KALI_IP> -Port 22
```

**Output:**
```text
TcpTestSucceeded : True
```

**Result:** SSH connectivity was successful.

## Important Observation

McAfee Personal Firewall was displayed as managing firewall settings in Windows Defender Firewall during testing. This may affect firewall rule enforcement.

## Conclusion

Successfully configured firewall rules on Windows and Kali Linux. Initially, Telnet port 23 was accessible. After configuration, the final connection test returned `Connection refused`, while SSH connectivity on port 22 remained successful.

## Report

Refer to `TASK 4 WIN FW CONFIG AND PORT BLOCKING.pdf for screenshots and detailed testing evidence.

**Note:** IP addresses have been hidden for privacy.
