# Incident Analysis — SSH Brute Force Detection

## Incident Summary

During monitoring of SSH authentication activity in the Splunk SOC lab, multiple failed authentication attempts were detected against the SSH server.

The activity originated from source IP address `192.168.64.1` and targeted the username `fakeuser`.

Splunk identified six failed authentication attempts within approximately one minute, exceeding the configured threshold of five failed attempts.

## Detection Details

- Detection: SSH Brute Force Detection
- Source IP: `192.168.64.1`
- Target Username: `fakeuser`
- Failed Attempts: 6
- Log Source: `/var/log/auth.log`
- Splunk Index: `security`
- Authentication Service: SSH
- Detection Threshold: 5 failed attempts

## Investigation

The authentication logs were analyzed in Splunk using SPL.

The investigation extracted the source IP address and username from the raw SSH authentication events and grouped the events by source IP and username.

The following behavior was observed:

1. Multiple failed SSH authentication attempts originated from the same source IP.
2. The attempts targeted the same username.
3. Six failed authentication attempts occurred within approximately one minute.
4. The activity exceeded the configured brute-force threshold.
5. Splunk generated a detection result for the activity.

## Detection Logic

The primary SPL detection aggregates failed authentication events by source IP and username.

A potential brute-force attack is identified when:

`failed_attempts >= 5`

This threshold allows repeated authentication failures to be separated from isolated login mistakes.

## Security Analysis

Repeated SSH authentication failures from a single source may indicate:

- Password guessing
- Credential brute forcing
- Unauthorized access attempts
- Automated authentication scanning

In a production SOC environment, the source IP would require additional investigation before determining whether the activity was malicious.

## MITRE ATT&CK Mapping

The activity is consistent with:

**T1110 — Brute Force**

T1110 covers attempts by adversaries to gain access to accounts through repeated authentication attempts.

## Recommended Response

If similar activity were detected in a production environment, the SOC analyst should:

1. Validate the source IP and affected account.
2. Review successful logins associated with the same source.
3. Determine whether the source IP is expected or authorized.
4. Review surrounding authentication and system activity.
5. Temporarily block the source IP if malicious activity is confirmed.
6. Reset or secure the affected account if compromise is suspected.
7. Escalate the incident according to the organization's incident response procedures.

## Conclusion

The detection successfully identified repeated SSH authentication failures exceeding the configured threshold.

This demonstrates the use of Splunk for log analysis, field extraction, detection engineering, alerting, and SOC investigation.
