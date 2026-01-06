# Scheduled Task Execution – HR User Investigation

## Objective
Identify which user from the HR department executed scheduled task activity that may indicate persistence or malicious behavior on a compromised host.

## Query Used
```spl
index=win_eventlogs schtasks HostName=HR_02 CommandLine="/create /tn OfficUpdater /tr \"C:\\Users\\Chris.fort\\AppData\\Local\\Temp\\update.exe\" /sc onstart"

```
## Time Range / Filters Applied

Investigation focused on March 2022, configured via the Splunk UI.

The query filters for:

schtasks to identify scheduled task creation activity

HostName=HR_02 to scope the investigation to the HR department

A specific CommandLine indicating task creation and execution of a suspicious binary

## Explanation

index=win_eventlogs specifies the Windows Event Logs index containing process creation events (Event ID 4688).

The keyword schtasks was used to identify executions related to Windows scheduled task management.

The CommandLine parameter reveals the creation of a scheduled task named OfficUpdater, configured to run a binary from a temporary user directory.

The task was set to execute on system startup, a common persistence technique used by attackers.

Analysis of the user context associated with this activity showed that Chris.fort was the only HR department user responsible for executing this scheduled task.

## Result

HR user observed executing scheduled tasks: Chris.fort

Suspicious scheduled task name: OfficUpdater

Executed binary path:
C:\Users\Chris.fort\AppData\Local\Temp\update.exe

Persistence mechanism: Scheduled task set to run on startup

## Why This Matters

Scheduled task abuse is a well-known persistence technique used by attackers to maintain access to compromised systems. Identifying unauthorized or suspicious task creation helps SOC analysts:

Detect persistence mechanisms early

Attribute malicious activity to specific users or hosts

Contain and remediate compromised accounts and endpoints

This finding confirms malicious post-compromise behavior originating from an HR department user account and supports further investigation into payload execution and command-and-control activity.
