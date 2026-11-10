# SOC Analyst – Class 02
## SOC Team Roles, Security Technologies, SIEM Fundamentals, Architecture & Alert Investigation

**Course:** SOC Analyst – Level 01  
**Class:** 02  
**Topic:** People, Process, Technology, SIEM, Security Tools, Architecture and Alert Handling

---

## 1. The Three Pillars of a SOC

A Security Operations Center (SOC) works through three major components:

1. **People** – The security professionals who monitor, detect, investigate and respond to threats.
2. **Process / Policy** – The documented procedures, rules and guidelines that define how security work is performed.
3. **Technology** – The tools and platforms used to collect data, detect threats, investigate incidents and respond.

A successful SOC needs all three. Buying security tools alone does not create an effective SOC.

---

## 2. SOC Team Structure and Roles

SOC teams are commonly organized into different operational levels. Exact responsibilities vary between organizations.

### 2.1 SOC Analyst – Level 1 (L1)

**Primary responsibility:** Alert monitoring and initial triage.

Typical tasks:

- Monitor SIEM dashboards and security alerts.
- Review incoming alerts and identify priority.
- Perform initial investigation using available logs and context.
- Identify obvious false positives and suspicious activity.
- Create and update incident tickets.
- Escalate confirmed or complex incidents to L2.
- Follow documented playbooks and escalation procedures.

**Example:** An alert reports multiple failed SSH login attempts. The L1 analyst checks source IP, target server, timestamps and number of failures, then escalates if suspicious.

### 2.2 SOC Analyst – Level 2 (L2)

**Primary responsibility:** Deeper investigation and incident handling.

Typical tasks:

- Investigate escalated L1 alerts.
- Correlate events from multiple sources.
- Identify affected users, endpoints, servers and systems.
- Determine likely attack technique and scope.
- Recommend or perform approved containment actions.
- Collect evidence and document findings.
- Escalate advanced incidents to L3 when required.

**Example:** L2 correlates firewall, Windows authentication and endpoint logs to determine whether repeated login failures were followed by a successful login.

### 2.3 SOC Analyst – Level 3 (L3)

**Primary responsibility:** Advanced threat analysis, threat hunting and detection improvement.

Typical tasks:

- Investigate complex and high-impact incidents.
- Perform proactive threat hunting.
- Analyze malware, attacker behavior and advanced techniques.
- Develop and improve detection logic.
- Map attacker behavior to MITRE ATT&CK.
- Lead technical incident response.
- Provide recommendations to improve security controls.

**Example:** L3 investigates possible lateral movement across several servers and develops a detection rule to identify similar behavior in the future.

### 2.4 SOC Manager

**Primary responsibility:** SOC operations, people, performance and governance.

Typical tasks:

- Manage SOC analysts and daily operations.
- Define workflows, policies and escalation procedures.
- Monitor SOC performance and service-level agreements (SLAs).
- Prepare management reports and risk summaries.
- Coordinate with IT, infrastructure, compliance and management teams.
- Plan staffing, training and technology requirements.

### 2.5 SIEM Engineer

A **SIEM Engineer** designs, builds, maintains and improves the SIEM platform and its detection capabilities.

Typical tasks:

- Design and maintain SIEM architecture.
- Integrate logs from endpoints, servers, firewalls, applications and cloud services.
- Develop and maintain log parsers, decoders and normalization.
- Create, tune and test detection rules and correlation logic.
- Manage SIEM performance, storage, indexing and scalability.
- Troubleshoot ingestion and alert-generation problems.
- Automate SIEM operations through APIs and scripts.
- Maintain integrations, dashboards and platform health.

### 2.6 SOC Analyst vs. SIEM Engineer

| SOC Analyst | SIEM Engineer |
|---|---|
| Uses SIEM to monitor and investigate | Builds and maintains SIEM |
| Analyzes alerts | Engineers detection rules and pipelines |
| Investigates incidents | Improves log ingestion and correlation |
| Follows response playbooks | Develops and maintains SIEM integrations |

---

## 3. The Technology Pillar: Security Tools

A SOC uses multiple tools to protect an organization's environment.

### 3.1 Popular SIEM Platforms

| SIEM Platform | Provider / Project | General Category |
|---|---|---|
| Splunk Enterprise Security | Splunk | Commercial |
| Microsoft Sentinel | Microsoft | Cloud-native SIEM |
| IBM QRadar | IBM | Commercial |
| Elastic Security | Elastic | Commercial and open-source components |
| Wazuh | Wazuh | Open-source security platform |

> **Note:** Product licensing and feature availability can vary by edition and deployment model. Elastic Stack is not identical to Elastic Security, and Wazuh is not simply another name for ELK.

### 3.2 Enterprise and Open-Source Options

**Commercial / enterprise platforms**
- Splunk Enterprise Security
- Microsoft Sentinel
- IBM QRadar

**Open-source or open-core options**
- Wazuh
- OpenSearch
- Elastic Stack components (subject to their respective licenses)

Organizations select platforms based on requirements such as budget, log volume, integrations, scalability, compliance and operational skills.

---

## 4. SOC Security Architecture (High-Level)

This section shows how a practical SOC is structured from data collection to response.

### 4.1 Architecture Layers

1. **Data Sources Layer**
- Endpoints (Windows, Linux, macOS)
- Network devices (firewall, VPN, routers, switches)
- Identity systems (Active Directory, IAM)
- Applications and databases
- Cloud services (IaaS, SaaS, PaaS)

2. **Collection & Transport Layer**
- Agents, syslog, API connectors, cloud collectors
- Secure transport channels (TLS, authenticated ingestion)
- Queues/buffers for reliability where needed

3. **Processing & Detection Layer**
- Parsing and normalization
- Enrichment (asset criticality, geo-IP, threat intel)
- Correlation and detection rules
- Behavioral analytics (where available)

4. **Storage & Search Layer**
- Indexed event storage
- Retention policies (hot/warm/cold tiers)
- Fast search for investigations and reporting

5. **Operations & Response Layer**
- Analyst triage and investigations
- Case management and ticketing
- Incident response workflows
- Reporting, metrics and governance

### 4.2 End-to-End SOC Data Flow

```text
Log Sources (Endpoint/Network/Cloud/App)
                |
                v
      Collectors / Agents / APIs
                |
                v
   SIEM Ingestion + Parsing + Normalization
                |
                v
 Correlation Rules + Detection Analytics
                |
                v
           Alerts / Cases
                |
                v
     L1 Triage -> L2 Investigation -> L3 Response
                |
                v
         Containment / Recovery / Lessons Learned
```

### 4.3 Why Architecture Matters

- Reduces blind spots by covering multiple telemetry sources.
- Improves alert quality through normalization and enrichment.
- Supports faster investigations through searchable, centralized data.
- Creates clear ownership between analysts, engineers and management.

---

## 5. Understanding Wazuh Architecture

Wazuh is an open-source security platform that provides capabilities such as security monitoring, threat detection, file integrity monitoring, vulnerability detection and compliance monitoring.

A typical Wazuh deployment includes:

1. **Wazuh Agent** – Installed on monitored endpoints and servers. It collects configured security data and sends it to the manager.
2. **Wazuh Manager** – Receives and analyzes agent data, uses decoders and rules, and generates alerts.
3. **Wazuh Indexer** – Stores and indexes alert and security data for searching and analysis. It is based on OpenSearch.
4. **Wazuh Dashboard** – Provides a web interface for searching, monitoring and investigating security data.

### Simplified Wazuh architecture

```text
Windows / Linux / macOS Endpoints
              |
         Wazuh Agent
              |
       Secure Communication
              |
        Wazuh Manager
       (Decoders + Rules)
              |
         Alerts / Events
              |
        Wazuh Indexer
          (OpenSearch)
              |
        Wazuh Dashboard
       (Search & Analysis)
```

### Important distinctions

- Wazuh Manager performs analysis using decoders and rules.
- Wazuh Indexer is the storage and indexing layer.
- Wazuh Dashboard is the user interface.
- OpenSearch is related to Elasticsearch historically, but they are separate projects.
- Kibana is associated with Elasticsearch; Wazuh Dashboard is the interface used in the Wazuh platform.

---

## 6. What Is SIEM?

**SIEM** stands for **Security Information and Event Management**.

A SIEM is a cybersecurity technology that collects, centralizes, processes, normalizes, analyzes and correlates logs and security events from multiple sources to help detect suspicious activities and support security monitoring and incident investigation.

### Key SIEM activities

- **Collect:** Receive logs from different sources.
- **Centralize:** Bring log data into a common platform.
- **Parse:** Extract useful fields from raw log messages.
- **Normalize:** Convert different log formats into a consistent structure.
- **Analyze:** Examine events for suspicious behavior.
- **Correlate:** Connect related events across systems and time.
- **Alert:** Generate notifications when detection conditions are met.
- **Investigate:** Help analysts understand what happened and its impact.

### Example log sources

- Windows and Linux servers
- Workstations and laptops
- Firewalls and routers
- Switches and VPN gateways
- Active Directory
- Web servers and applications
- Databases
- Endpoint security tools
- Cloud platforms

---

## 7. SIM and SEM: Understanding the Terms

Historically, SIEM combines two concepts:

### 7.1 SIM – Security Information Management

SIM focuses on the collection, storage, retention and management of security log information.

Examples:
- Centralized log storage
- Log retention
- Search and reporting
- Compliance reporting

### 7.2 SEM – Security Event Management

SEM focuses on real-time security event monitoring, analysis, correlation and alerting.

Examples:
- Detecting repeated failed logins
- Correlating events from multiple systems
- Generating security alerts
- Supporting incident response

**Remember:** SIEM combines log information management with security event monitoring and analysis. Modern SIEM platforms often provide both capabilities in one product.

---

## 8. Database Basics: SQL and NoSQL

SIEM platforms use storage and indexing technologies to manage large volumes of event data.

### SQL Database

A relational database stores data in structured tables with rows and columns.

Example:

| student_ID | student_name |
|---|---|
| 1234 | Nazim |
| 1235 | Rahim |

Example SQL query:

```sql
SELECT student_ID, student_name
FROM students
WHERE student_ID = 1234;
```

### NoSQL Database

NoSQL databases support data models beyond traditional relational tables. Depending on the database, data may be stored as documents, key-value pairs, wide-column records or graphs.

Example JSON document:

```json
{
  "student_ID": 1234,
  "student_name": "Nazim"
}
```

**Why this matters in SIEM:** Security events often arrive in different formats and at high volume. Search and indexing systems such as OpenSearch are commonly used to store and query SIEM data.

---

## 9. Core Functions of a SIEM

| Function | Meaning |
|---|---|
| Log Collection | Collect logs from different sources |
| Log Processing | Parse, enrich and normalize incoming logs |
| Event Analysis | Examine events for suspicious patterns |
| Correlation | Connect related events from multiple sources |
| Alert Generation | Create alerts when detection conditions match |
| Threat Detection | Identify potentially malicious behavior |
| Event Management | Track, prioritize and manage security events |
| Investigation Support | Provide context and evidence for analysts |
| Reporting | Produce operational, security and compliance reports |

### SIEM data flow

```text
Log Sources
    |
    v
Log Collection
    |
    v
Parsing and Decoding
    |
    v
Normalization / Enrichment
    |
    v
Detection Rules and Correlation
    |
    v
Alert Generation
    |
    v
Analyst Investigation
    |
    v
Close / Escalate / Respond
```

---

## 10. How Wazuh Detects an Attack

Consider a Windows endpoint generating security events.

```text
Windows Endpoint
       |
       v
Wazuh Agent
       |
       v
Wazuh Manager
       |
       v
Decoder: Understands the log format
       |
       v
Rule: Evaluates the event or event pattern
       |
       v
Alert: Generated when a rule matches
       |
       v
SOC Analyst: Investigates the alert
       |
       v
Close / Escalate / Response
```

### Important concepts

- **Raw Log:** The original log message received from a source.
- **Decoder:** Extracts fields and identifies the structure of a log.
- **Rule:** Defines conditions for identifying an event or suspicious behavior.
- **Alert:** A detection result generated when a rule condition is satisfied.
- **Investigation:** The analyst process for validating and understanding the alert.

A rule match does not automatically prove that an attack occurred. Analysts must validate evidence and context.

---

## 11. Alert Investigation Workflow

A common SOC workflow is:

1. **Receive** – An alert arrives in the SIEM.
2. **Triage** – Check severity, source, destination, timestamp and alert details.
3. **Investigate** – Review related logs and gather context.
4. **Classify** – Determine whether the activity is malicious, benign or inconclusive.
5. **Respond** – Take approved actions or recommend containment.
6. **Escalate** – Send complex or high-impact incidents to the appropriate team.
7. **Document** – Record evidence, analysis and actions.
8. **Close** – Close the case when the required process is complete.

### Alert classification terms

| Term | Meaning |
|---|---|
| TP – True Positive | The alert correctly identifies malicious or genuinely suspicious activity |
| FP – False Positive | The alert identifies activity as suspicious, but investigation shows it is benign |
| TN – True Negative | Benign activity is correctly treated as benign; generally not represented as an alert |
| FN – False Negative | Malicious activity occurs but the detection system fails to alert |

**Important:** A True Positive means detection is valid. It does not necessarily mean the incident is fully resolved.

### Example workflow

```text
Agent / Log Source
       |
       v
SIEM Detection Rule
       |
       v
Alert Generated
       |
       v
Analyst Investigation
       |
       +---- Benign activity ----> Close as False Positive
       |
       +---- Confirmed threat ---> Respond / Escalate
       |
       +---- Not enough evidence -> Gather more evidence
```

---

## 12. Practical Scenario: SSH Brute-Force Detection

**Scenario:** A Linux server receives 100 failed SSH login attempts from one source IP within a short period.

### What is SSH?

**SSH (Secure Shell)** is a network protocol used to securely access and manage remote systems, commonly Linux servers.

### What is a brute-force attack?

A brute-force attack attempts many possible passwords or credentials to gain unauthorized access.

### Example event

```text
Source IP: 192.0.2.50
Destination: Linux Server
Service: SSH
Failed login attempts: 100
Time window: 5 minutes
Result: Repeated authentication failures
```

The IP address above is a documentation example address, not a real attacker indicator.

### L1 analyst investigation checklist

- Confirm source and destination IP addresses.
- Verify number of failed login attempts and time range.
- Check which usernames were targeted.
- Identify whether source IP is internal or external.
- Search for a successful SSH login after failures.
- Review firewall, VPN and Linux authentication logs.
- Check whether source IP is an approved scanner or administrator system.
- Record evidence and escalate according to SOC playbook.

### Possible outcomes

- **Authorized testing:** Document approved activity and close according to policy.
- **Suspicious repeated failures:** Escalate for further investigation.
- **Successful login after repeated failures:** Treat as higher priority and investigate account, source and follow-on activity.
- **Confirmed compromise:** Follow incident response and containment procedures.

**Do not automatically block an IP or disable an account without authorization and an approved response procedure.**

---

## 13. EPP, EDR and XDR

These technologies support endpoint and broader security monitoring.

### EPP – Endpoint Protection Platform

EPP focuses on preventing endpoint threats.

Common capabilities:
- Anti-malware
- Antivirus
- Exploit prevention
- Host firewall
- Device control

### EDR – Endpoint Detection and Response

EDR focuses on monitoring endpoint activity, detecting suspicious behavior and supporting investigation and response.

Common capabilities:
- Endpoint telemetry collection
- Behavioral detection
- Process and command-line investigation
- Threat hunting
- Endpoint isolation or other response actions

### XDR – Extended Detection and Response

XDR correlates security data across multiple security layers, such as endpoints, email, identity, network and cloud environments, to provide broader detection and investigation.

| Technology | Main focus |
|---|---|
| EPP | Prevent endpoint threats |
| EDR | Detect and respond to endpoint threats |
| XDR | Correlate and respond across multiple security layers |
| SIEM | Centralize, analyze and correlate security events from many sources |

These technologies can complement one another; they are not necessarily replacements for each other.

---

## 14. Definitions (Expanded Glossary)

| Term | Definition |
|---|---|
| SOC | Security Operations Center: team and capability for continuous security monitoring and response |
| SIEM | Security Information and Event Management |
| SIM | Security Information Management |
| SEM | Security Event Management |
| Telemetry | Technical data emitted by systems, endpoints and applications for monitoring and analysis |
| Use Case | A specific detection objective (for example, brute-force login detection) |
| IOC | Indicator of Compromise, such as malicious IP, hash, domain or process behavior |
| TTP | Tactics, Techniques and Procedures used by threat actors |
| Log | A record of an event generated by a system or application |
| Event | An observable action or occurrence in a system |
| Parsing | Extracting meaningful fields from raw data |
| Normalization | Converting different log formats into a consistent structure |
| Correlation | Connecting related events to identify a meaningful pattern |
| Enrichment | Adding context to events (asset owner, criticality, geo-IP, threat intel, etc.) |
| Decoder | Component that interprets log structure and extracts fields |
| Detection Rule | Logic used to identify defined suspicious activity |
| Alert | Notification generated when a detection condition matches |
| Triage | Initial review and prioritization of an alert |
| Escalation | Sending an issue to a higher level or responsible team |
| Incident | A confirmed or suspected security event that requires response |
| Incident Response | Coordinated handling of a security incident |
| Brute Force | Repeated attempts to guess credentials |
| False Positive | Alert looks malicious but is actually benign after investigation |
| False Negative | Malicious activity that was not detected by controls |

---

## 15. Class 02 Review Questions

1. What are the three pillars of a SOC?
2. What are the main responsibilities of an L1 SOC Analyst?
3. What is the difference between L1, L2 and L3?
4. What does a SOC Manager do?
5. What is the role of a SIEM Engineer?
6. What does SIEM stand for?
7. What are the main functions of a SIEM?
8. What is the difference between SIM and SEM?
9. What is the difference between SQL and NoSQL?
10. What are the main components of Wazuh architecture?
11. What is the difference between a Wazuh Manager and Wazuh Indexer?
12. What is a decoder?
13. What is a detection rule?
14. What is the difference between a True Positive and False Positive?
15. What should an analyst investigate after 100 failed SSH login attempts?
16. What is the difference between EPP, EDR, XDR and SIEM?

---

## 16. Practical Homework

### Task 1 – SOC Roles

Write down at least five responsibilities each for:
- SOC Analyst L1
- SOC Analyst L2
- SOC Analyst L3
- SOC Manager
- SIEM Engineer

### Task 2 – SIEM Architecture

Draw a diagram showing how logs move from a Windows or Linux endpoint to the Wazuh Dashboard.

Include:
- Log source
- Wazuh Agent
- Wazuh Manager
- Decoder
- Detection Rule
- Wazuh Indexer
- Wazuh Dashboard
- SOC Analyst

### Task 3 – Alert Investigation

Scenario: A Linux server records 100 failed SSH login attempts within five minutes.

Write a short investigation report containing:
- Alert title and severity
- Source and destination
- Evidence collected
- Investigation steps
- Classification (TP, FP or inconclusive)
- Recommended action
- Escalation decision

### Task 4 – GitHub Notes

Create a Markdown file in your SOC learning repository:

```text
SOC-Analyst-Level-01/
└── Class-02/
    └── SOC-Class-02.md
```

Commit your class notes and practical homework to GitHub.

---

## 17. Class Summary

By the end of Class 02, students should understand:

- How people, process and technology work together in a SOC.
- The responsibilities of L1, L2, L3, SOC Manager and SIEM Engineer.
- Popular SIEM platforms and the basic Wazuh architecture.
- The meaning and core functions of SIEM.
- The difference between SIM and SEM.
- How logs are collected, processed, analyzed and converted into alerts.
- How SOC analysts investigate and classify alerts.
- The basic differences between EPP, EDR, XDR and SIEM.
- How to approach a repeated SSH authentication failure scenario.

**Next class:** Log Fundamentals – logs, events, event sources, log formats, timestamps, severity levels and Windows/Linux log sources.
