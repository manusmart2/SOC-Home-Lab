# Wazuh Agent Setup

This document describes how Wazuh agents are deployed, configured, connected to the Wazuh Manager, and verified in the home lab.

The purpose of the setup is to monitor endpoint activity and forward security telemetry to the centralized Wazuh Manager.

---

## 🏗️ Setup Overview

```text
                 Wazuh Manager
                      │
             Agent Registration
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
     Agent 01      Agent 02      Agent 03
        │             │             │
        └─────────────┼─────────────┘
                      │
                   Agent 04
```

The lab currently contains **four Wazuh agents**.

---

# 1. Agent Installation

The Wazuh agent is installed on the endpoint that needs to be monitored.

The installation method depends on the operating system.

## Windows

The Wazuh agent is installed on Windows endpoints and configured to communicate with the Wazuh Manager.

After installation, the Wazuh service can be checked with PowerShell:

```powershell
Get-Service WazuhSvc
```

A running service should show a status similar to:

```text
Status   Name       DisplayName
------   ----       -----------
Running  WazuhSvc   Wazuh
```

---

## Linux

On Linux endpoints, the Wazuh agent service can be checked with:

```bash
sudo systemctl status wazuh-agent
```

The service can be started with:

```bash
sudo systemctl start wazuh-agent
```

To enable it during system startup:

```bash
sudo systemctl enable wazuh-agent
```

---

# 2. Manager Configuration

The Wazuh agent needs to know the IP address or hostname of the Wazuh Manager.

The configuration contains the manager address used for agent communication.

Conceptually:

```text
Agent
  │
  │ Security Events
  │
  ▼
Wazuh Manager IP
  │
  ▼
Wazuh Manager
```

> **Security Note:** Actual IP addresses, credentials, registration keys, and other sensitive information should not be committed to this repository.

Use placeholders in documentation:

```text
<WAZUH_MANAGER_IP>
```

---

# 3. Network Connectivity

Before troubleshooting the Wazuh agent itself, network connectivity should be verified.

From the endpoint:

```bash
ping <WAZUH_MANAGER_IP>
```

For Windows:

```powershell
Test-Connection <WAZUH_MANAGER_IP>
```

Successful connectivity indicates that the endpoint can reach the manager at the basic network level.

---

# 4. Agent Service Verification

After configuring the agent, verify that the Wazuh service is running.

### Windows

```powershell
Get-Service WazuhSvc
```

### Linux

```bash
sudo systemctl status wazuh-agent
```

If the service is stopped, start it and verify the status again.

---

# 5. Verify Agent in Wazuh

After the agent is configured and running, the Wazuh Dashboard can be used to verify the connection.

Expected workflow:

```text
Agent Configuration
        │
        ▼
Agent Service Started
        │
        ▼
Network Connectivity
        │
        ▼
Manager Communication
        │
        ▼
Agent Appears Online
        │
        ▼
Events Begin Arriving
```

The important distinction is:

**Agent Online ≠ Events Successfully Collected**

An agent can appear online while the expected logs are not being collected. Therefore, event verification is also required.

---

# 6. Event Verification

After confirming that the agent is online, generate controlled activity on the endpoint.

Examples:

* User authentication
* Process execution
* PowerShell commands
* File activity
* System events

Then check the Wazuh Dashboard for the resulting events.

The validation process is:

```text
Generate Activity
       │
       ▼
Endpoint Creates Event
       │
       ▼
Wazuh Agent Collects Event
       │
       ▼
Wazuh Manager Receives Event
       │
       ▼
Event Appears in Dashboard
```

---

# 7. Troubleshooting

## Problem: Agent Offline

Initial checks:

### Check network connectivity

Windows:

```powershell
Test-Connection <WAZUH_MANAGER_IP>
```

Linux:

```bash
ping <WAZUH_MANAGER_IP>
```

### Check the agent service

Windows:

```powershell
Get-Service WazuhSvc
```

Linux:

```bash
sudo systemctl status wazuh-agent
```

### Check configuration

Verify that the configured manager address is correct.

---

## Problem: Agent Online but No Events

This requires a different investigation.

Check:

1. Agent service status
2. Agent configuration
3. Configured log sources
4. Windows Event Channels
5. Linux log files
6. Network connectivity
7. Wazuh Manager status
8. Wazuh Dashboard filters

The investigation should determine whether the problem is:

```text
Endpoint
   ↓
Log Generation
   ↓
Agent Collection
   ↓
Network Transport
   ↓
Manager Processing
   ↓
Dashboard
```

---

# 8. Lessons Learned

Working with multiple agents highlighted several important concepts:

* Agent connectivity depends on correct network configuration.
* A running agent service does not necessarily mean logs are being collected.
* Network troubleshooting should be performed before assuming a Wazuh configuration problem.
* Endpoint log sources must be correctly configured.
* Security monitoring requires validating the complete event pipeline.
* Troubleshooting is an important part of maintaining a SIEM environment.

---

# 9. Future Improvements

Planned improvements to the agent environment include:

* Document each endpoint individually
* Add screenshots of agent status
* Document Windows Event Channel configuration
* Add Sysmon integration
* Create custom detection rules
* Document common agent troubleshooting scenarios
* Add Linux-specific monitoring
* Create an agent deployment checklist

---

## ⚠️ Security Considerations

Never commit the following to GitHub:

```text
Passwords
API Keys
Authentication Keys
Private Keys
Agent Registration Keys
Personal IP Addresses
VPN Credentials
Cloud Credentials
Access Tokens
```

Use placeholders such as:

```text
<WAZUH_MANAGER_IP>
<AGENT_NAME>
<USERNAME>
```

when documenting the configuration.
