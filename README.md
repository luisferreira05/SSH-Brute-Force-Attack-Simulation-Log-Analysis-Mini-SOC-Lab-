# SSH Brute Force Attack Simulation & Log Analysis (Mini SOC Lab)

## Project Overview

This project simulates an SSH brute force attack in a controlled virtual lab environment and analyzes authentication logs from a SOC (Security Operations Center) perspective.

The objective was not exploitation, but understanding how brute force attacks appear in system logs and how they can be detected and mitigated.

---

## Lab Architecture

Attacker Machine:
- OS: Linux Mint (Host)
- Tool: Hydra

Target Machine:
- OS: Ubuntu Server 24.04 LTS
- Service: OpenSSH (Port 22)

Network:
- VirtualBox Host-Only Network
- Isolated internal environment

Simplified Architecture:

[ Attacker Machine ]  --->  [ Ubuntu Server ]
        |                          |
      Hydra                     SSH Service

---

## Attack Simulation

Tool used:
- Hydra (SSH brute force tool)

Command executed:

hydra -l target_user -P passwords.txt ssh://TARGET_IP

A custom wordlist was used to simulate multiple authentication attempts within a short time window.

### Attack Execution

![Attack Execution](screenshots/SSH_Brute_Force_Simulation_Execution)

---

## Log Analysis

Authentication logs were analyzed using:

sudo cat /var/log/auth.log | grep "Failed password"

### Observed Pattern

- Multiple failed login attempts
- Same source IP address
- Same targeted username
- Attempts occurred within seconds
- No successful authentication observed

### Log Evidence and Confirmation of No Successful Login

![Failed Attempts](screenshots/failed_attempts.png)

---

## Findings

- Source IP: Internal attacker machine
- Target User: target_user
- Failed Attempts: 9
- Successful Logins: 0
- Time Window: Less than 5 seconds
- Pattern indicates automated brute force attempt

---

## Risk Assessment

If this occurred in a production environment:

- Weak passwords could result in full system compromise
- Repeated login attempts could indicate automated scanning
- Attack could escalate if credentials were discovered

---

## Recommended Mitigations

- Implement Fail2Ban
- Disable password authentication
- Enforce SSH key-based authentication
- Restrict SSH access by IP
- Monitor authentication logs continuously

---

## Skills Demonstrated

- Linux system administration
- SSH service configuration
- Brute force attack simulation
- Log-based incident analysis
- Threat detection pattern recognition
- SOC-oriented thinking
