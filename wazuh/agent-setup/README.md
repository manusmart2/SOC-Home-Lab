# Wazuh Agent Deployment & Setup

This document describes how Wazuh agents were deployed and connected to the Wazuh Manager in my home lab.

The deployment process involved transferring the Wazuh Agent MSI package to the Windows endpoint, accessing the endpoint through RDP, installing the agent, configuring the Wazuh Manager address, applying the agent authentication key, and verifying the agent connection.

---

# 🏗️ Deployment Architecture

```text
                    Debian Wazuh Server
                    192.168.29.153
                           │
                           │
                     Wazuh Manager
                           │
                           │
              ┌────────────┴────────────┐
              │                         │
              ▼                         ▼
       Windows 7 Agent           Windows 8.1 Agent
       192.168.29.28             192.168.29.197
```

---

# 🧰 Deployment Tools

The following technologies were used during deployment:

| Tool / Technology | Purpose                            |
| ----------------- | ---------------------------------- |
| SSH               | Remote access / file transfer      |
| SCP               | Transfer Wazuh Agent MSI           |
| RDP               | Remote graphical access to Windows |
| MSI Installer     | Install Wazuh Agent                |
| Wazuh Agent       | Endpoint monitoring                |
| Wazuh Manager     | Centralized security monitoring    |

---

# 1. Prepare the Wazuh Agent Package

The Wazuh Agent MSI installer was prepared on the Debian/Linux environment.

The installer was then transferred to the Windows endpoint using SSH-based file transfer.

The general transfer workflow was:

```text
Debian Wazuh Server
       │
       │ SCP / SSH
       ▼
Windows Endpoint
       │
       ▼
Wazuh Agent MSI
```

The actual installer filename/version is intentionally not hard-coded in this documentation so that the repository remains accurate if the lab is rebuilt with a different Wazuh version.

---

# 2. Transfer the MSI to Windows

The Wazuh Agent MSI package was transferred to the Windows endpoint.

The transfer was performed using SSH/SCP.

Conceptually:

```text
Linux
  │
  │ Secure File Transfer
  ▼
Windows 7
```

Example SCP syntax:

```bash
scp wazuh-agent.msi <username>@<WINDOWS_IP>:/path/to/destination/
```

> The exact command may vary depending on the SSH/SCP configuration of the Windows endpoint.

---

# 3. Access Windows Endpoint Using RDP

After transferring the MSI file, the Windows endpoint was accessed using Remote Desktop Protocol.

The RDP session was used to perform the Wazuh Agent installation and configuration through the Windows graphical interface.

```text
RDP Client
    │
    ▼
Windows 7
192.168.29.28
    │
    ▼
Install Wazuh Agent
```

---

# 4. Install Wazuh Agent

The transferred MSI installer was executed on the Windows endpoint.

The installation process consisted of:

1. Open the transferred MSI package
2. Start the Wazuh Agent installation
3. Follow the installation wizard
4. Complete the installation
5. Configure the Wazuh Manager connection

---

# 5. Configure Wazuh Manager

During the agent configuration, the Wazuh Manager address was specified.

The Wazuh Manager in the lab is:

```text
Wazuh Manager
192.168.29.153
```

The communication path is:

```text
Windows Agent
      │
      │ Security Telemetry
      ▼
192.168.29.153
      │
      ▼
Wazuh Manager
```

---

# 6. Agent Authentication

The Wazuh Agent was configured using an agent authentication/enrollment key.

The key allows the endpoint to authenticate with the Wazuh Manager.

For security reasons, the actual key is **not stored in this repository**.

Documentation should use:

```text
<AGENT_AUTHENTICATION_KEY>
```

instead of the real value.

### Security Rule

Never commit:

```text
Agent Authentication Key
Passwords
API Keys
Private Keys
Access Tokens
```

to a public GitHub repository.

---

# 7. Start the Wazuh Agent

After installation and configuration, the Wazuh Agent service was started.

On Windows, the service can be checked using PowerShell:

```powershell
Get-Service WazuhSvc
```

Expected result:

```text
Status
------
Running
```

The service can be restarted with:

```powershell
Restart-Service WazuhSvc
```

---

# 8. Verify Network Connectivity

Before troubleshooting the Wazuh application layer, basic network connectivity was verified.

From Windows:

```powershell
Test-Connection 192.168.29.153
```

This confirms basic connectivity between the Windows endpoint and the Wazuh Server.

---

# 9. Verify Agent Status

After configuration and service startup, the Wazuh Dashboard was used to verify the agent status.

Expected workflow:

```text
Agent Installed
      │
      ▼
Manager IP Configured
      │
      ▼
Authentication Key Configured
      │
      ▼
Agent Service Started
      │
      ▼
Network Communication
      │
      ▼
Agent Appears Online
```

---

# 10. Windows 7 Agent

## Endpoint Information

| Property            | Value            |
| ------------------- | ---------------- |
| Operating System    | Windows 7        |
| IP Address          | `192.168.29.28`  |
| Wazuh Manager       | `192.168.29.153` |
| Agent Software      | Wazuh Agent      |
| Installation Method | MSI              |
| Remote Access       | RDP              |
| File Transfer       | SSH/SCP          |

### Deployment Flow

```text
Debian Server
     │
     │ SCP
     ▼
Wazuh Agent MSI
     │
     ▼
Windows 7
192.168.29.28
     │
     │ RDP
     ▼
MSI Installation
     │
     ▼
Manager IP Configuration
     │
     ▼
Agent Authentication
     │
     ▼
Wazuh Manager
192.168.29.153
```

---

# 11. Windows 8.1 Agent

The Windows 8.1 system is also configured as a Wazuh endpoint.

## Endpoint Information

| Property              | Value            |
| --------------------- | ---------------- |
| Operating System      | Windows 8.1      |
| IP Address            | `192.168.29.197` |
| Wazuh Manager         | `192.168.29.153` |
| Agent Software        | Wazuh Agent      |
| Additional Monitoring | Sysmon           |
| Remote Access         | RDP / SSH        |

The Windows 8.1 endpoint provides additional security telemetry through Sysmon.

```text
Windows 8.1
192.168.29.197
       │
       ├── Wazuh Agent
       │
       ├── Sysmon
       │
       ├── SSH
       │
       └── RDP
              │
              ▼
       Wazuh Manager
       192.168.29.153
```

---

# 12. Validate Event Collection

After confirming that the agent is online, the next step is to verify that events are actually being collected.

The validation process is:

```text
Generate Activity
       │
       ▼
Windows Event
       │
       ▼
Wazuh Agent
       │
       ▼
Wazuh Manager
       │
       ▼
Wazuh Dashboard
```

Examples of activities that can be used for validation:

* User authentication
* Failed authentication
* Process execution
* PowerShell activity
* File activity
* System events
* Sysmon events

---

# 13. Troubleshooting

If the agent appears offline:

### Check the service

```powershell
Get-Service WazuhSvc
```

### Check network connectivity

```powershell
Test-Connection 192.168.29.153
```

### Verify Manager Address

Confirm that the configured Manager address is:

```text
192.168.29.153
```

### Verify Authentication

Confirm that the correct agent authentication/enrollment information was configured.

---

# 14. Lessons Learned

This deployment provided practical experience with the complete endpoint onboarding process:

```text
Prepare
   ↓
Transfer
   ↓
Install
   ↓
Configure
   ↓
Authenticate
   ↓
Connect
   ↓
Verify
   ↓
Monitor
```

Key lessons:

* Secure file transfer can be used to stage software on lab endpoints.
* RDP can simplify Windows endpoint administration.
* Correct Manager addressing is essential for agent communication.
* Agent authentication is required for secure enrollment/communication.
* An online agent should be followed by event-generation testing.
* Network and service troubleshooting should be performed before deeper investigation.

---

# 🔐 Security Considerations

This repository is intended to document a home lab.

Never publish:

```text
❌ Wazuh Authentication Keys
❌ Passwords
❌ SSH Private Keys
❌ RDP Credentials
❌ API Keys
❌ Access Tokens
❌ Personal Credentials
```

Use placeholders:

```text
<WAZUH_MANAGER_IP>
<AGENT_AUTHENTICATION_KEY>
<USERNAME>
<PASSWORD>
```

Private lab IP addresses may also be replaced with placeholders if the repository is public.

---

# 🚀 Future Improvements

Planned improvements include:

* Document exact agent configuration
* Add screenshots of successful enrollment
* Document Windows Event Channel monitoring
* Document Sysmon integration
* Add agent troubleshooting cases
* Add custom Wazuh rules
* Document alert investigations
* Add endpoint-specific monitoring guides
