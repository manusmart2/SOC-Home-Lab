# Wazuh Troubleshooting

This document records troubleshooting performed while building and operating the Wazuh home lab.

The purpose is to document problems encountered, the investigation process, the solution, and the lessons learned.

---

# 1. Agent Connectivity Issue

## Problem

A Wazuh agent was not communicating correctly with the Wazuh Manager.

The first step was to determine whether the problem was related to:

* Network connectivity
* Virtual machine networking
* Wazuh agent service
* Manager connectivity
* Firewall
* Agent configuration

---

## Investigation

### Step 1 — Check IP Configuration

On Windows:

```powershell
ipconfig
```

On Linux:

```bash
ip addr
```

The endpoint IP address and Wazuh Manager IP address were identified.

---

### Step 2 — Test Connectivity

From the endpoint:

```powershell
Test-Connection <WAZUH_MANAGER_IP>
```

or:

```bash
ping <WAZUH_MANAGER_IP>
```

This helped determine whether the endpoint could communicate with the Wazuh Manager at the network level.

---

# 2. Virtual Machine Networking

## Problem

Virtual machine network configuration affected communication between the endpoints and the Wazuh Manager.

Different networking modes were evaluated, including:

```text
Host-Only
    +
Bridged
```

### Host-Only Network

Used to provide communication between virtual machines and the host in an isolated network.

```text
Host
 │
 ├── VM
 ├── VM
 └── VM
```

### Bridged Network

Used when a virtual machine needs to communicate through the physical network.

```text
Physical Network
       │
       ▼
    Host NIC
       │
       ▼
  Virtual Machine
```

---

# 3. Agent Service Verification

After network connectivity was checked, the Wazuh agent service was verified.

## Windows

```powershell
Get-Service WazuhSvc
```

Expected state:

```text
Running
```

If necessary, the service can be restarted:

```powershell
Restart-Service WazuhSvc
```

---

## Linux

```bash
sudo systemctl status wazuh-agent
```

Restart:

```bash
sudo systemctl restart wazuh-agent
```

---

# 4. Agent Online but No Events

## Problem

One of the important troubleshooting scenarios in the lab was an agent appearing **online**, while the expected security events were not immediately visible in the Wazuh Dashboard.

This demonstrated an important distinction:

```text
Agent Online
     ≠
Logs Successfully Collected
```

---

## Investigation Workflow

The following areas should be checked:

```text
Agent Status
     │
     ▼
Agent Configuration
     │
     ▼
Log Source Configuration
     │
     ▼
Endpoint Generates Event
     │
     ▼
Agent Collects Event
     │
     ▼
Manager Receives Event
     │
     ▼
Dashboard Displays Event
```

---

# 5. Windows Event Collection

For Windows endpoints, verify that the required Windows Event Channels are configured for collection.

Potential sources include:

```text
Security
System
Application
PowerShell
```

The exact event sources should be configured according to the monitoring requirements of the lab.

---

# 6. Validate Event Generation

Before troubleshooting Wazuh further, verify that the endpoint is actually generating the expected event.

For example:

```text
Endpoint Activity
       │
       ▼
Windows/Linux Event
       │
       ▼
Wazuh Agent
```

If the endpoint is not generating an event, the Wazuh agent cannot collect it.

This creates a useful troubleshooting distinction:

```text
No Event Generated
        ↓
Endpoint-side issue

Event Generated but Not Collected
        ↓
Agent/configuration issue

Event Collected but Not Visible
        ↓
Manager/dashboard/investigation issue
```

---

# 7. Connectivity Troubleshooting Checklist

When an agent is offline or events are missing, use the following checklist.

### Network

* [ ] Endpoint has a valid IP address
* [ ] Wazuh Manager has a reachable IP
* [ ] Endpoint can reach Manager
* [ ] VM network adapter is correctly configured
* [ ] Firewall is not blocking required communication

### Agent

* [ ] Wazuh agent is installed
* [ ] Wazuh service is running
* [ ] Manager address is correct
* [ ] Agent configuration is valid
* [ ] Agent is registered correctly

### Log Collection

* [ ] Required log source is configured
* [ ] Endpoint is generating events
* [ ] Agent is collecting the event
* [ ] Manager is receiving the event
* [ ] Dashboard filters are correct

---

# 8. Troubleshooting Methodology

Instead of changing multiple configurations at once, troubleshoot from the bottom of the stack upward.

```text
1. Hardware / VM
       ↓
2. Network
       ↓
3. Service
       ↓
4. Configuration
       ↓
5. Log Generation
       ↓
6. Log Collection
       ↓
7. Manager Processing
       ↓
8. Dashboard
```

This approach helps isolate the actual source of a problem.

---

# 9. Lessons Learned

The troubleshooting process provided several practical lessons:

### Networking Matters

A security monitoring system depends on reliable communication between endpoints and the manager.

### Online Status Is Not Enough

An agent being online does not guarantee that the required logs are being collected.

### Troubleshoot Systematically

Checking network connectivity, services, configuration, event generation, and log collection separately makes troubleshooting easier.

### Validate the Complete Pipeline

The final test should always confirm:

```text
Activity
   ↓
Event
   ↓
Agent
   ↓
Manager
   ↓
Alert
   ↓
Dashboard
```

---

# 🔐 Security Considerations

Do not publish sensitive infrastructure information.

Before committing troubleshooting screenshots or configuration files, remove or redact:

```text
Passwords
API Keys
Agent Authentication Keys
Private Keys
Access Tokens
Credentials
Personal Information
Sensitive IP Addresses
```

Use placeholders:

```text
<WAZUH_MANAGER_IP>
<AGENT_NAME>
<USERNAME>
```

---

# 🚀 Future Troubleshooting Documentation

Future entries will document:

* Wazuh service failures
* Agent registration problems
* Windows Event Log collection problems
* Linux log collection problems
* Firewall-related issues
* Network adapter issues
* Custom rule troubleshooting
* False positives
* Missing alerts
* Sysmon integration problems
