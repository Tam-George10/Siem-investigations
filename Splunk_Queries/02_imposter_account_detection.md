# Imposter Account Detection – Explanation

## Objective
Identify any suspicious or duplicate user accounts in the Windows process execution logs that could indicate potential impersonation or unauthorized access.

## Query Used
```spl
index=win_eventlogs | top limit=11 username

```
After identifying the suspicious username Amel1a, a second query was used to investigate activity for this account:
```spl
index=win_eventlogs username="Amel1a"

```
## Time Range / Filters Applied

Full log range for the investigation (March 2022) was applied in the Splunk UI.

The top limit=11 query was used to identify unusual or unexpected usernames that deviate from known accounts.

Filtering on username="Amel1a" allowed precise inspection of process execution activity for the suspicious account.

## Explanation

The first query lists the most frequent usernames observed in the logs, limited to the top 11, to quickly spot anomalies.

Amel1a was flagged because there is already a legitimate user Amelia. The slight difference suggests a potential imposter account.

The second query isolates all events for Amel1a, enabling further investigation into what processes were executed and when.

This approach allows SOC analysts to identify suspicious accounts and correlate them with potentially malicious activity in the network.

## Result

Suspicious user detected: Amel1a

Time of suspicious activity: Identified via filtering with username="Amel1a" in Splunk (3/5/22
12:54:30.000 PM).

## Why This Matters

Detecting imposter or duplicated accounts is critical in a SOC environment because:

It helps identify potential insider threats or account compromise.

Enables correlation of unauthorized activity with specific hosts or departments.

Provides actionable intelligence for alerting, blocking, or further investigation.

By documenting both the query and the results, this step demonstrates a structured method for detecting account impersonation within Windows logs.
