# SIEM Investigations – Suspicious Process Execution (HR Department)

This repository documents a hands-on SOC investigation focused on identifying, analyzing, and responding to suspicious process execution activity on a host within the HR department. The project follows a real-world Blue Team workflow, including log ingestion, alert triage, user behavior analysis, LOLBIN and scheduled task investigation, network activity analysis, and threat intelligence correlation.  

---

<h1>Investigation Scenario</h1>

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
  <li><b>Vim / Text Editors:</b> Used to review raw log data and extract relevant information</li>
</ul>

<h2>Utilities Used</h2>
<p>
<b>Oracle VirtualBox:</b> Provided an isolated sandbox environment for safely handling and analyzing potentially malicious files and logs.<br>
<b>Splunk Query Editor:</b> Used to build, test, and refine SPL queries for analysis of event logs and alert correlation.<br>
<b>Browser / Network Tools:</b> Standard browsers and online tools (e.g., VirusTotal, WHOIS lookups) used for network artifact investigation, IP reputation, and malware source verification.
</p>


---

<h2>Investigation Findings</h2>



### 1. Log Collection & Ingestion
- Total number of process execution logs ingested for March 2022: `[13,959]`  
- Query used: 📄 Detailed query explanation [see Splunk_Queries/01_log_ingestion_explained.md](Splunk_Queries/01_log_ingestion_explained.md)  
<p align="center">
  <img src="https://i.imgur.com/5DPiDwT.png" width="80%" alt="March Logs"/>
</p>

### 2. Alert Analysis & Initial Triage
- IDS alert identified suspicious process execution on HR host  
- Host: `[Amel1a]`  
- Timeline of suspicious processes: `[3/5/22
12:54:30.000 PM]`
  - Query used:📄 Detailed query explanation: [see Splunk_Queries/02_imposter_account_detection.md](Splunk_Queries/02_imposter_account_detection.md)

<p align="center">
  <img src="https://i.imgur.com/zKAHm1c.png" width="80%" alt="IDS Alert"/>
</p>
<p align="center">
  <img src="https://i.imgur.com/0GuScp6.png" width="80%" alt="IDS Alert"/>
</p>


### 3. User Activity Correlation
- HR user observed running scheduled tasks: `[Chris.fort]`
- Query used:📄 Detailed query explanation: [see Splunk_Queries/03_scheduled_task_hr_user.md](Splunk_Queries/03_scheduled_task_hr_user.md)

<p align="center">
  <img src="https://i.imgur.com/8HeRmVj.png" width="80%" alt="User Activity"/>
</p>

### 4. Scheduled Task & LOLBIN Investigation
- HR user executing LOLBIN to download payload: `[Haroon]`  
- System process (LOLBIN) used: `[Certutil.exe]`
- Query used:📄 Detailed query explanation: [see Splunk_Queries/04_lolbin_payload_download.md](Splunk_Queries/04_lolbin_payload_download.md) 
<p align="center">
  <img src="https://i.imgur.com/yY4GNKZ.png" width="80%" alt="LOLBIN Execution"/>
</p>

## 5. Network Analysis
- **Third-party site accessed:** `[Controlc.com]`
- **URL connected to by infected host:** `[hxxps://controlc[.]com/e4d11035
]`
- Query used:📄 Detailed query explanation: [see Splunk_Queries/05_Network_Analysis.md](Splunk_Queries/05_Network_Analysis.md) 

<p align="center">
  <img src="https://i.imgur.com/GN0HSRV.png" width="80%" alt="Network Activity"/>
</p>

## 6. Host Analysis
- **Date of binary execution:** `[3/4/22
10:38:28.000 AM]`
- **File saved on host from C2 server:** `[Benign.exe]`
- Query used:📄 Detailed query explanation: [see Splunk_Queries/06_Host_Analysis.md
](Splunk_Queries/06_Host_Analysis.md) 

<p align="center">
  <img src="https://i.imgur.com/bkEbQSQ.png" width="80%" alt="Host Activity"/>
</p>



---
<h3>Indicators of Compromise (IOCs)</h3>
<ul>
  <li><b>Imposter Username:</b> Amel1a</li>
  <li><b>Legitimate Lookalike Username:</b> Amelia</li>

  <li><b>Compromised User Account:</b> Chris.fort</li>
  <li><b>Compromised User Account:</b> Haroon</li>

  <li><b>Affected Department:</b> Human Resources (HR)</li>
  <li><b>Affected Host:</b> HR_02</li>

  <li><b>Malicious Scheduled Task Name:</b> OfficUpdater</li>
  <li><b>Persistence Mechanism:</b> Windows Scheduled Task (schtasks)</li>

  <li><b>LOLBIN Used:</b> certutil.exe</li>

  <li><b>Downloaded Payload Name:</b> Benign.exe</li>
  <li><b>Payload Execution Date:</b> 2022-03-04</li>
  <li><b>Payload Execution Time:</b> 10:38:28 AM</li>

  <li><b>External Domain Contacted:</b> controlc[.]com</li>
  <li><b>External URL:</b> https://controlc[.]com/e4d11035</li>

  <li><b>Payload Source Type:</b> Third-party file hosting service</li>
  <li><b>Log Source:</b> Windows Event Logs (Event ID 4688)</li>
</ul>


---


<h2>MITRE ATT&amp;CK Mapping</h2>
<ul>
  <li><b>T1036 – Masquerading</b><br/>
      Imposter account <code>Amel1a</code> closely resembled a legitimate user (<code>Amelia</code>), indicating account masquerading.</li>

  <li><b>T1078 – Valid Accounts</b><br/>
      Legitimate HR user accounts (<code>Chris.fort</code>, <code>Haroon</code>) were used to execute malicious activity.</li>

  <li><b>T1053.005 – Scheduled Task / Job: Scheduled Task</b><br/>
      A scheduled task (<code>OfficUpdater</code>) was created to establish persistence and execute a malicious binary on system startup.</li>

  <li><b>T1218 – Signed Binary Proxy Execution</b><br/>
      The native Windows utility <code>certutil.exe</code> was abused as a Living-off-the-Land Binary (LOLBIN) to bypass security controls.</li>

  <li><b>T1105 – Ingress Tool Transfer</b><br/>
      A payload (<code>benign.exe</code>) was downloaded from an external source using <code>certutil.exe</code>.</li>

  <li><b>T1071 – Application Layer Protocol</b><br/>
      The infected host communicated with a third-party web service (<code>controlc[.]com</code>) over HTTP(S).</li>
</ul>


---

<h2>Final Assessment &amp; Recommendations</h2>
<p>
The investigation confirmed that the suspicious process execution activity observed on HR department hosts was the result of a confirmed compromise. Analysis of Windows process creation logs (Event ID 4688) revealed multiple stages of attacker activity, including account masquerading, persistence establishment, and malicious payload delivery using native system utilities.
</p>

<p>
A suspicious imposter account (<code>Amel1a</code>) was identified during log review, closely resembling a legitimate user account (<code>Amelia</code>). Further analysis revealed abuse of valid HR user accounts (<code>Chris.fort</code> and <code>Haroon</code>) to execute scheduled tasks and download an external payload using the Living-off-the-Land Binary <code>certutil.exe</code>. The payload was retrieved from a third-party file-sharing service and successfully written to disk, confirming post-compromise activity.
</p>

<p>
Based on the collected evidence, the following actions are recommended:
</p>

<ul>
  <li><b>Immediate account remediation:</b> Reset passwords and review access privileges for all affected and related HR user accounts, including monitoring for additional imposter or lookalike usernames.</li>

  <li><b>Persistence eradication:</b> Identify and remove unauthorized scheduled tasks, including the task named <code>OfficUpdater</code>, and verify system startup configurations across HR hosts.</li>

  <li><b>LOLBIN abuse prevention:</b> Restrict or monitor execution of high-risk native utilities such as <code>certutil.exe</code> through endpoint protection policies and application control.</li>

  <li><b>Network containment:</b> Block identified external infrastructure, including the defanged domain <code>controlc[.]com</code> and associated URLs, at network security and proxy layers.</li>

  <li><b>Threat detection enhancement:</b> Create and deploy SIEM correlation rules to detect scheduled task creation, certutil-based downloads, and execution of binaries from temporary user directories.</li>

  <li><b>Post-incident monitoring:</b> Increase monitoring across HR department systems for signs of lateral movement, repeated execution attempts, or additional payload retrieval.</li>
</ul>

<h3>Case Closure</h3>
<p>
The incident was successfully investigated and documented using available host-based telemetry. All identified Indicators of Compromise (IOCs), including user accounts, command-line artifacts, file names, and external URLs, have been preserved for SOC reporting and future detection. The case has been formally closed following validation of findings and implementation of recommended remediation actions.
</p>

