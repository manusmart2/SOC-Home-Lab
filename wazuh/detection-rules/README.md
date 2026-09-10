# Wazuh Detection & Sysmon Monitoring

This section documents the detection capabilities being developed in my Wazuh home lab.

The primary focus is understanding how Windows and Sysmon telemetry can be collected by Wazuh and used for security detection and investigation.

---

# 🎯 Objectives

* Understand Wazuh detection rules
* Monitor Windows security activity
* Collect Sysmon telemetry
* Identify suspicious endpoint behavior
* Understand rule IDs and alert severity
* Investigate alerts generated from endpoint activity
* Develop custom detection rules
* Reduce false positives through testing.

---

# 🏗️ Detection Architecture

```text
                    Windows 8.1
                  192.168.29.197
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
        Windows Events           Sysmon
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                    Wazuh Agent
                         │
                         ▼
                   Wazuh Manager
                  192.168.29.153
                         │
                         ▼
                  Detection Rules
                         │
                         ▼
                       Alert
                         │
                         ▼
                 Wazuh Dashboard
```

---

# 🔬 Sysmon

Sysmon is enabled on the Windows 8.1 endpoint.

It provides additional telemetry that can help with endpoint investigations.

The Windows 8.1 endpoint:

```text
Operating System: Windows 8.1
IP Address:       192.168.29.197
```

Sysmon can provide visibility into activities such as:

* Process creation
* Process termination
* Network connections
* File creation
* Registry activity
* DNS activity
* Process relationships

This additional telemetry can provide useful context when investigating suspicious activity.

---

# 📥 Sysmon Event Collection

The basic collection pipeline is:

```text
Sysmon
  │
  │ Event
  ▼
Windows Event Log
  │
  ▼
Wazuh Agent
  │
  ▼
Wazuh Manager
  │
  ▼
Wazuh Detection Engine
  │
  ▼
Alert
```

A successful setup should allow relevant Sysmon events to be viewed and investigated through the Wazuh environment.

---

# 🧪 Detection Testing Methodology

Detection testing is performed using controlled activity inside the isolated home lab.

The general process is:

```text
1. Generate Controlled Activity
              ↓
2. Verify Windows/Sysmon Event
              ↓
3. Verify Wazuh Collection
              ↓
4. Check Wazuh Alert
              ↓
5. Investigate Event Details
              ↓
6. Determine Detection Quality
              ↓
7. Document Findings
```

---

# 🛡️ Detection Use Cases

The following use cases can be developed and tested in the lab.

---

## 1. Process Creation

### Objective

Monitor process creation and identify unusual process execution.

### Telemetry

Sysmon process creation events can provide information such as:

* Process name
* Process ID
* Parent process
* Command line
* User
* Executable path
* Timestamp

### Investigation

Questions:

* What process was executed?
* Who executed it?
* What was the parent process?
* What command line was used?
* Was the execution expected?
* Were other related processes created?

---

## 2. PowerShell Monitoring

### Objective

Monitor PowerShell execution and investigate suspicious command-line activity.

### Investigation Fields

Review:

```text
User
Process
Parent Process
Command Line
Timestamp
Host
Related Events
```

PowerShell activity should be analyzed in context rather than automatically treated as malicious.

---

## 3. Network Connection Monitoring

### Objective

Use Sysmon telemetry to investigate network connections created by processes.

### Investigation

Review:

* Source host
* Destination IP
* Destination port
* Process
* Process ID
* Timestamp
* User

Example investigation flow:

```text
Network Connection
       │
       ▼
Identify Process
       │
       ▼
Identify User
       │
       ▼
Review Command Line
       │
       ▼
Check Destination
       │
       ▼
Determine Risk
```

---

## 4. Authentication Monitoring

### Objective

Monitor authentication activity on Windows endpoints.

Potential investigation areas include:

* Successful logons
* Failed logons
* Remote logons
* Account activity
* Repeated authentication failures

Investigation should include:

```text
Username
Source IP
Destination Host
Timestamp
Logon Type
Authentication Result
```

---

## 5. File Activity

### Objective

Monitor changes to important files and directories.

Investigation fields may include:

* File path
* File name
* User
* Timestamp
* Hash
* Process responsible for the change

---

# 🧩 Custom Wazuh Rules

Custom rules can be developed when the default Wazuh rules do not provide the desired detection behavior.

A custom rule should have:

* Clear purpose
* Appropriate severity
* Relevant event conditions
* Minimal false positives
* Useful alert description

Example structure:

```xml
<group name="custom_detection,">
    <rule id="100001" level="7">
        <description>Custom detection example</description>
    </rule>
</group>
```

> This is only a structural example. Actual custom rules should be based on tested events from this lab.

---

# 📊 Rule Severity

Wazuh alerts have severity levels that help prioritize investigation.

A higher severity should generally indicate greater security relevance.

When creating custom rules, severity should be selected carefully.

Avoid assigning high severity to routine activity because excessive high-severity alerts can create alert fatigue.

---

# 🔎 Detection Investigation

When a detection triggers, investigate the event using the following approach:

```text
Alert
 │
 ├── What happened?
 │
 ├── Which endpoint?
 │
 ├── Which user?
 │
 ├── When?
 │
 ├── Which process?
 │
 ├── What command?
 │
 ├── What parent process?
 │
 ├── Any network connections?
 │
 ├── Any related events?
 │
 └── Is the activity expected?
```

---

# 🚦 Detection Classification

Each investigated alert can be classified as:

### Benign

Expected administrative or user activity.

### Suspicious

Activity that requires additional investigation.

### Malicious

Activity that provides sufficient evidence of compromise or malicious behavior within the lab scenario.

Example:

```text
Alert
  │
  ▼
Investigation
  │
  ├── Expected → Benign
  │
  └── Unexpected
          │
          ▼
      Investigate
          │
          ├── Insufficient Evidence → Suspicious
          │
          └── Confirmed Malicious → Malicious
```

---

# 📋 Detection Documentation Template

Use this template for every new detection added to the project:

```markdown
## Detection — <Detection Name>

### Objective

<What security behavior is being detected?>

### Endpoint

- Host:
- Operating System:
- IP:
- Agent:

### Data Source

- Windows Event Log:
- Sysmon:
- Other:

### Trigger

<What activity generates the detection?>

### Wazuh Rule

- Rule ID:
- Rule Level:
- Rule Description:

### Investigation

<How was the alert investigated?>

### Evidence

<Add sanitized logs or screenshots>

### Result

- Classification:
- False Positive:
- Detection Successful:

### Lessons Learned

<What was learned from testing?>

### Future Improvements

<What can be improved?>
```

---

# 📈 Detection Development Lifecycle

Detection development follows an iterative process:

```text
Idea
 │
 ▼
Create Test Scenario
 │
 ▼
Generate Event
 │
 ▼
Collect Telemetry
 │
 ▼
Create/Modify Detection
 │
 ▼
Test Detection
 │
 ▼
Investigate Alert
 │
 ▼
Evaluate False Positives
 │
 ▼
Improve Rule
 │
 ▼
Document
```

---

# 🚀 Planned Detection Improvements

Future work includes:

* Create custom Wazuh rules
* Expand Sysmon monitoring
* Develop PowerShell detections
* Develop suspicious process detections
* Monitor RDP authentication
* Monitor SSH authentication
* Create file-integrity detections
* Investigate parent/child process relationships
* Improve alert severity classification
* Document false-positive handling
* Build SOC investigation playbooks

---

# 🧠 Skills Demonstrated

This project demonstrates practical experience with:

* Wazuh
* SIEM detection
* Sysmon
* Windows Event Logs
* Endpoint telemetry
* Process analysis
* PowerShell monitoring
* Authentication monitoring
* Network-event investigation
* Custom detection development
* Alert triage
* False-positive analysis
* SOC investigation methodology
