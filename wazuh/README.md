# Wazuh Home Lab

This section documents my hands-on **Wazuh SIEM/XDR home lab**, including deployment, endpoint monitoring, log collection, alert generation, investigation, and troubleshooting.

The objective is to build practical experience with security monitoring and understand how endpoint activity is collected, analyzed, and presented as security alerts.

---

## 🏗️ Lab Overview

The Wazuh environment consists of:

* Wazuh Manager
* Wazuh Dashboard
* Wazuh Agents
* Windows endpoints
* Linux systems
* Virtual machines
* Isolated virtual networking

The current lab includes **four Wazuh agents** connected to the Wazuh Manager.

---

## 🔄 Monitoring Architecture

```text
                  Wazuh Home Lab
                        │
                        ▼
                ┌───────────────┐
                │ Wazuh Manager │
                └───────┬───────┘
                        │
                 Security Events
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
    Agent 01         Agent 02        Agent 03
        │               │               │
        └───────────────┼───────────────┘
                        │
                        ▼
                     Agent 04
                        │
                        ▼
                Wazuh Dashboard
                        │
                        ▼
               Alert Investigation
```

---

# 🎯 Objectives

The main objectives of this lab are:

* Deploy and configure Wazuh
* Connect multiple endpoints to the Wazuh Manager
* Monitor Windows and Linux systems
* Collect endpoint security logs
* Generate controlled security events
* Investigate Wazuh alerts
* Understand Wazuh detection capabilities
* Troubleshoot agent connectivity
* Practice SOC-style alert investigation
* Develop custom monitoring and detection capabilities

---

# 🖥️ Components

## Wazuh Manager

The Wazuh Manager provides centralized security monitoring.

Responsibilities include:

* Receiving events from agents
* Processing endpoint telemetry
* Applying detection rules
* Generating alerts
* Managing connected agents
* Providing security monitoring capabilities

---

## Wazuh Agents

The agents are installed on monitored endpoints.

They collect information such as:

* Windows event logs
* Linux logs
* Process activity
* File activity
* Authentication events
* System information
* Security-related events

The collected data is sent to the Wazuh Manager for analysis.

---

# 📊 Monitoring Workflow

The basic workflow in this lab is:

```text
Endpoint Activity
       │
       ▼
Log/Event Generated
       │
       ▼
Wazuh Agent
       │
       ▼
Wazuh Manager
       │
       ▼
Detection Rules
       │
       ▼
Security Alert
       │
       ▼
Wazuh Dashboard
       │
       ▼
Investigation
```

---

# 🔍 Security Monitoring

The lab is used to monitor activities including:

* User authentication
* Failed login attempts
* Process execution
* PowerShell activity
* Windows security events
* Linux system events
* File changes
* Suspicious commands
* Endpoint configuration changes
* Potential indicators of compromise

---

# 🧪 Alert Generation

Controlled activities are generated on the endpoints to verify that events are successfully collected by Wazuh.

Examples include:

```text
Authentication Activity
        ↓
Process Execution
        ↓
PowerShell Activity
        ↓
File/System Changes
        ↓
Security Events
```

The purpose is to validate the complete monitoring pipeline:

**Generate → Collect → Detect → Alert → Investigate**

---

# 🕵️ Alert Investigation

When an alert is generated, I investigate it using the Wazuh Dashboard and available endpoint logs.

The investigation process includes:

1. Identify the affected endpoint
2. Review the alert
3. Identify the event source
4. Review timestamp
5. Examine the username
6. Examine the process or command
7. Check related events
8. Determine whether the activity is expected or suspicious
9. Document findings
10. Identify possible indicators of compromise

---

# 🌐 Networking

The Wazuh lab uses virtual machine networking to allow communication between the manager and monitored endpoints.

Networking configurations used during lab development include:

* Host-only networking
* Bridged networking
* Static/dynamic IP addressing
* VM-to-VM communication

Basic connectivity testing:

```bash
ping <WAZUH_MANAGER_IP>
```

Agent communication problems are investigated by checking:

* IP configuration
* Network connectivity
* Wazuh services
* Firewall configuration
* Agent configuration
* Manager connectivity

---

# 🔧 Troubleshooting

Building the Wazuh environment has involved troubleshooting issues such as:

### Agent Offline

Possible checks:

```bash
ping <WAZUH_MANAGER_IP>
```

Check the Wazuh agent service on Linux:

```bash
sudo systemctl status wazuh-agent
```

On Windows, the Wazuh agent service can be checked through:

```powershell
Get-Service WazuhSvc
```

---

### Agent Online but No Events

Investigation areas include:

* Agent configuration
* Log collection configuration
* Windows Event Channel configuration
* Linux log sources
* Wazuh agent service
* Manager connectivity
* Firewall/network configuration

---

# 📁 Documentation

Detailed Wazuh documentation is organized into the following sections:

| Directory          | Purpose                                   |
| ------------------ | ----------------------------------------- |
| `architecture/`    | Lab architecture and topology             |
| `agent-setup/`     | Agent installation and configuration      |
| `detection-rules/` | Detection and custom rule documentation   |
| `alerts/`          | Alert-generation experiments and findings |
| `troubleshooting/` | Problems encountered and solutions        |

---

# 🚀 Future Improvements

Planned improvements include:

* Add more endpoint types
* Create custom Wazuh detection rules
* Improve Windows event monitoring
* Add Sysmon telemetry
* Create security dashboards
* Document common investigation procedures
* Build SOC investigation playbooks
* Add more controlled attack simulations
* Document false-positive analysis
* Improve alert correlation

---

# 📚 Skills Demonstrated

Through this project I am developing practical experience in:

* SIEM
* Wazuh
* Endpoint Detection and Monitoring
* Windows Security Monitoring
* Linux Monitoring
* Log Analysis
* Alert Investigation
* Incident Investigation
* Active Directory Monitoring
* PowerShell Monitoring
* Network Troubleshooting
* Security Event Analysis

````

### Next step

Commit this as:

```text
docs: add Wazuh lab overview
````

Then create the next directory:

```text
wazuh/agent-setup/
```

Inside it, create:

```text
README.md
```

**Don't fill it yet.** Once you've created that file, tell me **"created"**, and we'll document exactly how you configured your Wazuh agents, including the Windows agent setup and the connectivity troubleshooting you actually performed.
