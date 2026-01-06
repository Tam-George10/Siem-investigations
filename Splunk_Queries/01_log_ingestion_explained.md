```markdown
# Log Ingestion Query – Explanation

## Objective
Determine the total number of Windows process execution logs ingested into Splunk for the month of March 2022.

## Query Used
```spl
index=win_eventlogs
Time Range Applied
Start: 2022-03-01

End: 2022-03-31

The time range was configured directly in the Splunk UI to restrict results to events generated during March 2022.

Explanation
index=win_eventlogs specifies the Splunk index containing Windows Event Logs ingested from the environment.

This index includes Event ID 4688 process creation logs, which were the only logs available for analysis due to resource constraints.

By applying a time filter for March 2022, Splunk returns all process execution events generated during this period.

Splunk’s event counter was used to calculate the total number of matching logs.

Result
Total logs ingested for March 2022: 13,959

Why This Matters
Understanding log volume is a critical first step in SOC investigations. It helps analysts:

Validate successful log ingestion

Establish a baseline for activity

Identify abnormal spikes or drops in process execution events

This step confirms that sufficient telemetry was available to continue deeper investigation and threat analysis.
