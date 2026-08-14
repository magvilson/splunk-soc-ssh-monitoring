# Lab Setup

## Overview

This project demonstrates a Splunk-based SOC monitoring environment designed to detect and investigate suspicious SSH authentication activity.

The lab collects Linux SSH authentication logs, indexes them in Splunk, and uses SPL searches to identify failed logins, brute-force behavior, and suspicious authentication patterns.

## Environment

### Splunk Server
- Operating System: Ubuntu Server ARM64
- SIEM: Splunk Enterprise
- Hostname: soc-splunk-server
- Splunk Web Port: 8000
- Data Source: `/var/log/auth.log`
- Splunk Index: `security`

### Analyst Workstation
- macOS
- Web browser used to access Splunk Enterprise
- SSH used for remote administration of the Splunk server

## Data Collection

Linux authentication events from:

`/var/log/auth.log`

were monitored by Splunk and indexed into:

`index=security`

The authentication logs contain events such as:

- Failed SSH password attempts
- Successful SSH authentications
- Invalid user authentication attempts
- Source IP addresses
- Target usernames

## Detection Workflow

The project follows the following SOC workflow:

SSH Authentication Activity
        ↓
Linux `/var/log/auth.log`
        ↓
Splunk Data Ingestion
        ↓
SPL Detection Searches
        ↓
SSH Brute-Force Detection
        ↓
Splunk Alert
        ↓
SOC Investigation

## Detection Logic

Three primary searches were developed:

1. Failed SSH Login Investigation
2. SSH Brute-Force Detection
3. SSH Brute Force Followed by Successful Authentication

A brute-force condition is identified when a source IP generates five or more failed SSH authentication attempts against a user account.

## Dashboard

A Splunk dashboard named:

`SOC SSH Security Monitoring`

was created to provide visibility into:

- Total failed SSH logins
- Total successful SSH logins
- Brute-force detections
- SSH authentication activity over time
- Source IP activity
- Targeted user accounts

## Alerting

A scheduled Splunk alert named:

`SSH Brute Force Detection`

was configured to identify brute-force activity automatically.

The alert evaluates SSH authentication activity and triggers when the detection search returns one or more results.
