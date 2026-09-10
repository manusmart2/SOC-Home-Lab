# Wazuh Home Lab Architecture

This document describes the architecture of my Wazuh home lab, including the Wazuh Manager, monitored Windows endpoints, networking, and security telemetry flow.

The lab is built using virtual machines and is designed to provide hands-on experience with centralized security monitoring, endpoint telemetry, Windows security events, Sysmon, and remote administration services.

---

# 🏗️ Current Lab Architecture

```text
                         Home Lab Network
                         192.168.29.0/24
                                │
                ┌───────────────┴────────────────┐
                │                                │
                ▼                                ▼
       ┌─────────────────┐              ┌─────────────────┐
       │  Wazuh Server   │              │ Windows 7 Agent │
       │ Debian Server   │              │                 │
       │                 │              │ 192.168.29.28  │
       │ 192.168.29.153  │              │                 │
       │                 │              │ Wazuh Agent     │
       │ Wazuh Manager   │              └─────────────────┘
       └────────┬────────┘
                │
                │ Security Telemetry
                │
                ├──────────────────────────────┐
                │                              │
                ▼                              ▼
       ┌─────────────────┐
       │ Windows 8.1     │
       │ Agent           │
       │                 │
       │ 192.168.29.197  │
       │                 │
       │ Wazuh Agent     │
       │ Sysmon          │
       │ SSH             │
       │ RDP             │
       └─────────────────┘
```

---

# 🖥️ Infrastructure

## Wazuh Server

| Property         | Details                         |
| ---------------- | ------------------------------- |
| Operating System | Debian Server                   |
| IP Address       | `192.168.29.153`                |
| Role             | Wazuh Manager                   |
| Purpose          | Centralized security monitoring |

The Debian server acts as the central Wazuh Manager.

It receives telemetry from monitored endpoints and processes security events for detection and investigation.

---

# 🪟 Windows 7 Endpoint

| Property         | Details             |
| ---------------- | ------------------- |
| Operating System | Windows 7           |
| IP Address       | `192.168.29.28`     |
| Role             | Wazuh Agent         |
| Purpose          | Endpoint monitoring |

The Windows 7 system is monitored using the Wazuh Agent.

The endpoint provides Windows security telemetry that can be forwarded to the Wazuh Manager for centralized analysis.

---

# 🪟 Windows 8.1 Endpoint

| Property              | Details          |
| --------------------- | ---------------- |
| Operating System      | Windows 8.1      |
| IP Address            | `192.168.29.197` |
| Role                  | Wazuh Agent      |
| Additional Monitoring | Sysmon           |
| Remote Services       | SSH, RDP         |

The Windows 8.1 endpoint is used for more advanced security monitoring.

In addition to the Wazuh Agent, **Sysmon** is enabled to provide additional Windows telemetry.

SSH and RDP are also enabled on this endpoint to support remote administration and security-monitoring scenarios.

---

# 🔄 Security Telemetry Flow

The primary telemetry flow is:

```text
Windows Endpoint
      │
      │
      ├── Windows Security Events
      │
      ├── System Events
      │
      ├── Application Events
      │
      └── Sysmon Events
              │
              ▼
        Wazuh Agent
              │
              │ Security Telemetry
              ▼
       Wazuh Manager
       192.168.29.153
              │
              ▼
       Detection Rules
              │
              ▼
          Alerts
              │
              ▼
      Wazuh Dashboard
              │
              ▼
       Investigation
```

---

# 🔬 Sysmon Integration

Sysmon is enabled on the Windows 8.1 endpoint to provide additional visibility into Windows activity.

Sysmon can provide useful telemetry related to activities such as:

* Process creation
* Process termination
* Network connections
* File creation
* Driver activity
* Registry activity
* DNS activity
* Process relationships

This additional telemetry can improve the ability to investigate suspicious endpoint activity.

---

# 🌐 Network Layout

The current systems use the following network:

```text
Network: 192.168.29.0/24
```

### Hosts

```text
Wazuh Server
192.168.29.153
       │
       ├──────── Windows 7
       │          192.168.29.28
       │
       └──────── Windows 8.1
                  192.168.29.197
```

All three systems are currently identified within the same `192.168.29.0/24` network.

---

# 🔐 Remote Access

The Windows 8.1 endpoint has the following remote-access services enabled:

### SSH

SSH provides command-line remote administration.

Typical connection:

```bash
ssh <username>@192.168.29.197
```

### RDP

Remote Desktop Protocol provides graphical remote access to the Windows endpoint.

RDP activity can also provide useful security events for monitoring and investigation.

> Credentials should never be stored in this repository.

---

# 🛡️ Security Monitoring Use Cases

The current architecture can be used to investigate scenarios involving:

### Authentication

```text
Login Attempt
     ↓
Windows Event
     ↓
Wazuh Agent
     ↓
Wazuh Manager
     ↓
Alert / Investigation
```

### Process Execution

```text
Process Started
     ↓
Sysmon
     ↓
Wazuh Agent
     ↓
Wazuh Manager
     ↓
Detection
```

### Remote Access

```text
SSH / RDP Connection
        ↓
Authentication Events
        ↓
Windows Logs
        ↓
Wazuh Agent
        ↓
Wazuh Manager
        ↓
Investigation
```

---

# 🔎 Investigation Workflow

When investigating an event, the following workflow is used:

```text
Alert
  │
  ▼
Identify Endpoint
  │
  ▼
Identify User
  │
  ▼
Review Timestamp
  │
  ▼
Review Event Details
  │
  ▼
Check Related Events
  │
  ▼
Analyze Process / Network Activity
  │
  ▼
Determine:
Benign / Suspicious / Malicious
  │
  ▼
Document Findings
```

---

# 📊 Current Infrastructure Summary

| Component     | OS            | IP Address       | Security Telemetry   |
| ------------- | ------------- | ---------------- | -------------------- |
| Wazuh Manager | Debian Server | `192.168.29.153` | Wazuh Manager        |
| Endpoint 01   | Windows 7     | `192.168.29.28`  | Wazuh Agent          |
| Endpoint 02   | Windows 8.1   | `192.168.29.197` | Wazuh Agent + Sysmon |

---

# 🚀 Planned Architecture Improvements

Future improvements to the lab may include:

* Adding additional Windows endpoints
* Adding Linux endpoints
* Expanding Sysmon monitoring
* Creating custom Wazuh rules
* Monitoring SSH authentication
* Monitoring RDP authentication
* Creating dashboards for authentication events
* Adding network monitoring
* Documenting attack and detection scenarios
* Building incident-response playbooks

---

# ⚠️ Security & Privacy

This repository documents a private home lab.

Before publishing screenshots or configuration files, remove:

* Passwords
* API keys
* Authentication keys
* Private keys
* Access tokens
* Personal information
* Credentials

The IP addresses documented here are private RFC1918 addresses used within the lab environment.
