# SIEM Investigations – Suspicious Process Execution (HR Department)

This repository documents a hands-on SOC investigation focused on identifying, analyzing, and responding to suspicious process execution activity on a host within the HR department. The project follows a real-world Blue Team workflow, including log ingestion, alert triage, user behavior analysis, LOLBIN and scheduled task investigation, network activity analysis, and threat intelligence correlation.  

---

<h1>Real-World Scenario</h1>

An IDS alert indicated suspicious process execution on a host in the HR department, suggesting a potential compromise. Analysis revealed the execution of tools commonly associated with network information gathering and scheduled task creation, confirming malicious behavior.  

Due to limited resources, only **Windows process creation logs (Event ID 4688)** were collected and ingested into **Splunk** under the `win_eventlogs` index for further investigation.  

### Network Context

The organization’s network is divided into three departments:  

**IT Department**  
- James  
- Moin  
- Katrina  

**HR Department**  
- Haroon  
- Chris  
- Diana  

**Marketing Department**  
- Bell  
- Amelia  
- Deepak  

This segmentation was used to correlate suspicious activity to specific departments and users during the investigation.  

---

<h2>Description</h2>

<b>This project focuses on the full investigation and triage of suspicious process execution to determine whether it represents legitimate activity or malicious compromise within the HR department host.</b>
<br /><br />

<b>Investigation Workflow Overview:</b>
<ul>
  <li>🖥️ <b>Log Collection & Ingestion</b> – Gather Windows process execution logs (Event ID 4688) and ingest them into Splunk for structured analysis</li>
  <li>🔍 <b>Alert Analysis & Triage</b> – Review IDS alerts to identify hosts and processes of interest</li>
  <li>👤 <b>User Activity Correlation</b> – Map observed activity to network segmentation and departmental users to detect anomalous behavior and imposters</li>
  <li>🛠️ <b>Scheduled Task & LOLBIN Investigation</b> – Identify execution of Living-off-the-Land Binaries (LOLBINs) and scheduled tasks used for reconnaissance or payload delivery</li>
  <li>🌐 <b>Network & Host Analysis</b> – Determine which third-party sites were accessed, files downloaded, and endpoint artifacts present</li>
  <li>📂 <b>Artifact Collection & Evidence Documentation</b> – Extract downloaded files, record filenames, hashes, and other indicators of compromise for further triage</li>
  <li>📑 <b>Reporting & MITRE ATT&CK Mapping</b> – Document investigation findings, correlate with MITRE ATT&CK tactics and techniques, and provide actionable recommendations</li>
</ul>

All analysis was performed in a controlled environment to ensure safe handling of potentially malicious content.  

---

<h2>Tools Used</h2>
<ul>
  <li><b>Splunk:</b> SIEM platform used for log ingestion, querying, and correlation</li>
  <li><b>Windows Event Logs:</b> Primary source of process execution information (Event ID 4688)</li>
  <li><b>VirusTotal / Sandbox:</b> Used for analysis of downloaded payloads</li>
  <li><b>Vim / Text Editors:</b> Used to review raw log data and extract relevant information</li>
</ul>

---

<h2>Investigation Findings</h2>

> **Note:** Replace the placeholders below with your actual investigation details, screenshots, and queries.

### 1. Log Collection & Ingestion
- Total number of process execution logs ingested for March 2022: `[INSERT NUMBER]`  
- Query used: see [`queries/march_logs.txt`](queries/march_logs.txt)  
<p align="center">
  <img src="screenshots/march_logs.png" width="80%" alt="March Logs"/>
</p>

### 2. Alert Analysis & Triage
- IDS alert identified suspicious process execution on HR host  
- Host: `[INSERT HOSTNAME]`  
- Timeline of suspicious processes: `[INSERT TIMELINE]`  
<p align="center">
  <img src="screenshots/ids_alert.png" width="80%" alt="IDS Alert"/>
</p>

### 3. User Activity Correlation
- Potential imposter account detected: `[INSERT USERNAME]`  
- HR user observed running scheduled tasks: `[INSERT USERNAME]`  
<p align="center">
  <img src="screenshots/user_activity.png" width="80%" alt="User Activity"/>
</p>

### 4. Scheduled Task & LOLBIN Investigation
- HR user executing LOLBIN to download payload: `[INSERT USERNAME]`  
- System process (LOLBIN) used: `[INSERT LOLBIN NAME]`  
<p align="center">
  <img src="screenshots/lolbin_execution.png" width="80%" alt="LOLBIN Execution"/>
</p>

### 5. Network & Host Analysis
- Date of binary execution: `[YYYY-MM-DD]`  
- Third-party site accessed: `[INSERT SITE]`  
- File saved on host from C2 server: `[INSERT FILENAME]`  
- Malicious pattern/flag: `THM{..........}`  
- URL connected to by infected host: `[INSERT URL]`  
<p align="center">
  <img src="screenshots/network_activity.png" width="80%" alt="Network Activity"/>
</p>

### 6. Artifact Collection & Documentation
- SHA256 hash of downloaded file: `[INSERT HASH]`  
- File size: `[INSERT SIZE]`  
- Actual file extension: `[INSERT EXTENSION]`  
<p align="center">
  <img src="screenshots/artifact.png" width="80%" alt="Downloaded Artifact"/>
</p>

---

<h2>MITRE ATT&amp;CK Mapping</h2>
<ul>
  <li><b>T1110 – Brute Force</b> (if applicable)</li>
  <li><b>T1078 – Valid Accounts</b> (if accounts compromised)</li>
  <li><b>T1059 – Command and Scripting Interpreter</b> (if LOLBIN/script used)</li>
  <li><b>T1071 – Application Layer Protocol</b> (network-based data exfiltration)</li>
</ul>

---

<h2>Final Assessment & Recommendations</h2>
<p>
The investigation confirmed that suspicious process execution on the HR host represented malicious activity, including potential credential compromise and unauthorized payload download. Recommendations include:
</p>
<ul>
  <li>Immediate password reset and account lockout for affected users</li>
  <li>Blocking suspicious LOLBIN execution via endpoint controls</li>
  <li>Blocking source IPs and domains in network security devices</li>
  <li>Monitoring for additional anomalies across HR department hosts</li>
  <li>Documenting indicators of compromise for SIEM correlation</li>
</ul>

---

<h3>Case Closure</h3>
<p>
The incident was successfully investigated, documented, and mitigated. All findings, logs, and artifacts have been preserved for SOC reporting and compliance purposes.
</p>
