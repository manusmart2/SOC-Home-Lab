# Wazuh Alert Generation & Investigation

This section documents controlled security activities performed in the Wazuh home lab and the alerts generated from those activities.

The objective is to understand how endpoint activity is converted into security telemetry, detected by Wazuh, and investigated by an analyst.

---

## 🎯 Objectives

* Generate controlled security events
* Verify that Wazuh agents collect the events
* Analyze Wazuh alerts
* Understand alert severity and detection rules
* Identify the affected endpoint
* Investigate the source of an alert
* Correlate related events
* Document investigation findings

---

# 🔄 Alert Investigation Workflow

```text
Controlled Activity
        │
        ▼
Endpoint Generates Event
        │
        ▼
Wazuh Agent Collects Event
        │
        ▼
Wazuh Manager Processes Event
        │
        ▼
Detection Rule Matches
        │
        ▼
Wazuh Alert Generated
        │
        ▼
Dashboard Investigation
        │
        ▼
Determine:
Benign / Suspicious / Malicious
```

---

# 🧪 Alert Testing Methodology

Each alert investigation follows a consistent process.

### Step 1 — Generate Activity

Perform a controlled action on a lab endpoint.

Examples:

* Authentication activity
* Process execution
* PowerShell activity
* File modification
* Configuration changes

### Step 2 — Verify Event Collection

Confirm that the endpoint generated the expected event and that the Wazuh agent is communicating with the manager.

### Step 3 — Locate the Alert

Search for the corresponding event in the Wazuh Dashboard.

### Step 4 — Analyze the Alert

Review:

* Timestamp
* Agent
* Rule ID
* Rule description
* Severity
* Username
* Source IP
* Destination IP
* Process
* Command
* Event source

### Step 5 — Investigate

Determine whether the activity is:

```text
Expected
   │
   ├── Yes → Benign Activity
   │
   └── No  → Continue Investigation
                  │
                  ▼
            Suspicious Activity
```

### Step 6 — Document

Record the activity, evidence, analysis, and conclusion.

---

# 🚨 Alert Examples

The following sections will be populated as additional alerts are generated and investigated.

---

## Alert 01 — Authentication Activity

### Objective

Generate controlled authentication activity and observe how it is recorded by Wazuh.

### Activity

```text
[Document the activity performed here]
```

### Expected Evidence

```text
[Add relevant event details]
```

### Wazuh Detection

| Field     | Value          |
| --------- | -------------- |
| Agent     | `<AGENT_NAME>` |
| Rule ID   | `<RULE_ID>`    |
| Severity  | `<LEVEL>`      |
| Timestamp | `<TIMESTAMP>`  |
| User      | `<USERNAME>`   |
| Source IP | `<SOURCE_IP>`  |

### Investigation

Questions considered:

* Who performed the activity?
* Which endpoint was involved?
* Was the authentication successful?
* Was there a failed authentication attempt?
* What was the source IP?
* Were there related authentication events?
* Is the behavior expected?

### Conclusion

```text
[Document investigation conclusion]
```

---

# Alert 02 — PowerShell Activity

### Objective

Observe PowerShell execution on a Windows endpoint and identify the resulting security telemetry.

### Activity

```powershell
[Document controlled PowerShell activity here]
```

### Investigation

Review:

* PowerShell process creation
* Parent process
* User account
* Command line
* Timestamp
* Related Windows events
* Related Wazuh alerts

### Evidence

```text
[Add sanitized event information here]
```

### Conclusion

```text
[Document whether the activity was expected or suspicious]
```

---

# Alert 03 — Process Execution

### Objective

Monitor process creation and understand how endpoint process activity appears in Wazuh.

### Activity

```text
[Document process/activity performed here]
```

### Investigation

Analyze:

* Process name
* Process ID
* Parent process
* Executable path
* User
* Command line
* Execution time

### Evidence

```text
[Add sanitized evidence]
```

### Conclusion

```text
[Document findings]
```

---

# Alert 04 — File Activity

### Objective

Monitor controlled file activity and evaluate the resulting Wazuh telemetry.

### Activity

```text
[Document file activity here]
```

### Investigation

Review:

* File path
* File name
* File hash
* User
* Action performed
* Timestamp
* Related alerts

### Evidence

```text
[Add evidence]
```

### Conclusion

```text
[Document findings]
```

---

# 📋 Investigation Record Template

Use the following template whenever a new alert investigation is added.

```markdown
## Alert — <Alert Name>

### Objective

<What are you testing?>

### Activity

<What activity was generated?>

### Endpoint

- Host:
- Operating System:
- Agent:
- IP:

### Alert Details

- Rule ID:
- Rule Description:
- Severity:
- Timestamp:
- User:
- Source IP:

### Investigation

<What did you investigate?>

### Evidence

<Add sanitized logs/screenshots/details>

### Analysis

<Explain what the evidence means>

### Conclusion

<Benign / Suspicious / Malicious>

### Lessons Learned

<What did you learn from this investigation?>
```

---

# 🔎 Analyst Investigation Questions

For every alert, consider the following questions:

### Who?

```text
Which user or account generated the activity?
```

### What?

```text
What happened?
```

### When?

```text
When did the activity occur?
```

### Where?

```text
Which endpoint was affected?
```

### How?

```text
How was the activity performed?
```

### Why?

```text
Was the activity expected?
```

### What Next?

```text
Are there related events that require further investigation?
```

---

# 🧩 Alert Correlation

Individual alerts should not always be investigated in isolation.

For example:

```text
PowerShell Execution
        │
        ▼
Process Creation
        │
        ▼
File Modification
        │
        ▼
Network Connection
        │
        ▼
Potential Security Incident
```

Correlating multiple events can provide more context than analyzing a single alert.

---

# 📸 Evidence

Screenshots can be stored in:

```text
screenshots/
```

Recommended evidence includes:

* Wazuh alert details
* Agent status
* Event details
* Windows Event Viewer
* Process information
* Relevant logs

Before uploading screenshots, remove or redact:

* Passwords
* API keys
* Authentication tokens
* Private keys
* Personal information
* Sensitive network information

---

# 📈 Future Alert Investigations

Planned investigations include:

* Failed authentication attempts
* Successful authentication events
* PowerShell execution
* Suspicious process execution
* File integrity events
* User account changes
* Scheduled task activity
* Windows security events
* Sysmon events
* Suspicious network activity
* Custom Wazuh detection rules

---

# 🧠 Skills Demonstrated

This project section demonstrates practical experience with:

* SIEM alert monitoring
* Security event analysis
* Wazuh
* Windows Event Logs
* PowerShell monitoring
* Process analysis
* Authentication monitoring
* File integrity monitoring
* IOC identification
* Alert correlation
* SOC-style investigation
* Security documentation
