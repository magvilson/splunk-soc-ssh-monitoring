# Splunk SOC SSH Monitoring & Brute-Force Detection Lab

## Overview

This project demonstrates the implementation of a small Security Operations Center (SOC) monitoring environment using Splunk Enterprise.

The lab collects Linux SSH authentication logs, ingests them into Splunk, and uses Splunk Processing Language (SPL) searches to detect suspicious authentication activity.

The project demonstrates practical skills in:

- Security monitoring
- Log ingestion and analysis
- Splunk Enterprise
- Splunk Processing Language (SPL)
- SSH authentication analysis
- Brute-force detection
- Alert creation
- Incident investigation
- SOC documentation

---

## Lab Architecture

The environment consists of:

**Splunk Server**
- Ubuntu Server ARM64
- Splunk Enterprise
- Hostname: `soc-splunk-server`
- Splunk Web: Port `8000`
- Monitored log: `/var/log/auth.log`
- Splunk index: `security`

**Analyst Workstation**
- macOS
- Web browser used to access Splunk Enterprise
- Terminal used to generate SSH authentication activity

### Data Flow

```text
SSH Authentication Activity
          |
          v
 /var/log/auth.log
          |
          v
   Splunk Enterprise
          |
          v
    security index
          |
          v
      SPL Searches
          |
          v
 Brute-Force Detection
          |
          v
      Splunk Alert
          |
          v
  SOC Investigation
```

---

## Advanced Detection — Brute Force Followed by Successful Login

A second detection was developed to identify a potentially higher-risk scenario where repeated authentication failures are followed by a successful SSH authentication.

The SPL detection is available at:

`detections/ssh-brute-force-followed-by-success.spl`

---

## Repository Structure

```text
splunk-soc-ssh-monitoring/
├── README.md
├── .gitignore
├── detections/
│   ├── failed-ssh-logins.spl
│   ├── ssh-brute-force.spl
│   └── ssh-brute-force-followed-by-success.spl
├── docs/
│   ├── lab-setup.md
│   └── incident-analysis.md
└── screenshots/
    ├── 01-failed-ssh-events.png
    ├── 02-brute-force-detection.png
    ├── 03-alert-configuration.png
    └── 04-alert-triggered.png
```

---

## Incident Analysis

The detected activity originated from `192.168.64.1` and targeted the username `fakeuser`.

Six failed authentication attempts occurred within approximately one minute. Because the activity exceeded the configured threshold of five failed attempts, Splunk generated a detection result and triggered the configured alert.

In a production SOC environment, investigative steps would include:

- Reviewing additional authentication activity from the source IP
- Determining whether the source IP is expected or suspicious
- Checking for successful authentication after the failures
- Reviewing activity associated with the targeted account
- Correlating the source with other security logs
- Escalating the incident if compromise is suspected

---

## MITRE ATT&CK Mapping

**T1110 — Brute Force**

This technique involves repeated authentication attempts to gain unauthorized access to an account or system.

---

## Skills Demonstrated

- Splunk Enterprise
- Linux log monitoring
- SSH authentication analysis
- SPL query development
- Regular-expression field extraction
- Security event correlation
- Threshold-based detection
- Alert configuration
- Incident investigation
- SOC documentation

---

## Project Purpose

This project was created as a hands-on cybersecurity portfolio lab demonstrating practical SOC analyst skills including log collection, detection engineering, alerting, investigation, and documentation.

The environment is a controlled lab. All authentication activity shown in this repository was generated for educational and defensive security testing purposes.
