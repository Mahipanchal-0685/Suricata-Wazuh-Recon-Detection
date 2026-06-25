# Suricata-Wazuh Recon Detection

## Overview

This project demonstrates the detection of Nmap reconnaissance activity using Suricata IDS and Wazuh.

A Kali Linux VM was used to perform an Nmap scan against an Ubuntu server running Suricata and Wazuh. Suricata generated alerts which were processed and visualized in the Wazuh Dashboard.

---

## Environment

* Kali Linux VM (Attacker)
* Ubuntu VM (Target & Monitoring)
* Suricata IDS
* Wazuh Manager
* Wazuh Dashboard

---

## Detection Workflow

```text
Nmap Scan
    ↓
Suricata Detection
    ↓
eve.json Alert
    ↓
Wazuh Processing
    ↓
Wazuh Dashboard Alert
```

---

## Challenges Solved

### Issue 1: Suricata Service Failure

Suricata failed to start because the configured rule path did not match the actual generated rule location.

### Issue 2: No Events in eve.json

Suricata was monitoring the wrong network interface (`eth0` instead of `enp0s3`).

### Resolution

The rule path and monitored interface were corrected, allowing Suricata to successfully detect and log network activity.

---

## Results

* Successfully simulated reconnaissance activity using Nmap
* Generated Suricata alerts
* Verified events in `eve.json`
* Integrated alerts with Wazuh
* Confirmed alert visibility in the Wazuh Dashboard

---

## Evidence

### Nmap Reconnaissance Scan

![Nmap Scan](screenshots/01-nmap-scan.png)

### Detection in Wazuh Dashboard

![Wazuh Dashboard Alert](screenshots/02-wazuh-dashboard-alert.png)

### Suricata Alert Events

![eve.json Alerts](screenshots/03-eve-json-alerts.png)

### Wazuh Log Ingestion

![Wazuh Log Ingestion](screenshots/04-wazuh-log-ingestion.png)

---

## Skills Demonstrated

* Linux Administration
* Network Monitoring
* Intrusion Detection
* Security Event Analysis
* Wazuh SIEM Operations
* Troubleshooting and Log Analysis
