# Host Analysis – Binary Execution and Artifact Identification

## Objective
Determine the execution date of the malicious binary and identify the file saved on the host during post-exploitation activity.

## Query Used
```spl
index=win_eventlogs *certutil*

```

## Time Range / Filters Applied

Investigation scoped to March 2022, configured via the Splunk UI.

The query filters for events containing certutil, which was previously identified as the LOLBIN used for payload download.

## Explanation

index=win_eventlogs specifies the Windows Event Logs index containing process creation events (Event ID 4688).

Filtering on *certutil* allows identification of all executions involving the abused system utility.

Event timestamps associated with the process execution were used to determine when the binary was executed on the compromised host.

The CommandLine field revealed the output file name written to disk during the download operation.

This analysis allows attribution of execution timing and confirmation of artifact creation using only host-based logs.

## Result

Date of binary execution: 2022-03-04 10:38:28 AM

File saved on host from C2 server: benign.exe

## Why This Matters

Determining execution timing and identifying dropped artifacts are critical steps in incident response. This information enables SOC analysts to:

Build an accurate incident timeline

Identify files for containment and eradication

Support forensic analysis and endpoint remediation

This finding confirms successful payload download and execution on the infected host and supports continued investigation into command-and-control activity.
