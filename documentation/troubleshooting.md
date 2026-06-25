# Troubleshooting Guide

## Issue 1: Suricata Failed to Start

### Cause

The configured Suricata rule path did not match the actual location where the rules were generated.

### Investigation

Check the Suricata service status:

```bash
sudo systemctl status suricata
```

Validate the Suricata configuration:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

Verify where the rules were stored:

```bash
sudo ls -l /var/lib/suricata/rules/
```

```bash
sudo ls -l /etc/suricata/rules/
```

### Resolution

Updated the rule path inside the Suricata configuration and restarted the service.

```bash
sudo systemctl restart suricata
```

Verify the service is running:

```bash
sudo systemctl status suricata
```

---

## Issue 2: No Events Appeared in eve.json

### Cause

Suricata was monitoring the wrong network interface (`eth0` instead of `enp0s3`).

### Investigation

Identify the active network interface:

```bash
ip a
```

Verify the interface configured in Suricata:

```bash
sudo grep -n "interface:" /etc/suricata/suricata.yaml
```

Monitor Suricata events:

```bash
sudo tail -f /var/log/suricata/eve.json
```

Generate test traffic from the attacker machine:

```bash
sudo nmap -A <target-ip>
```

### Resolution

Updated the monitored interface in the Suricata configuration and restarted the service.

```bash
sudo systemctl restart suricata
```

Verify that events are being generated:

```bash
sudo tail -f /var/log/suricata/eve.json
```

---

## Issue 3: Alerts Not Visible in Wazuh

### Cause

Although Suricata was generating alerts, they were not initially visible within the Wazuh Dashboard.

### Investigation

Verify the Wazuh Manager status:

```bash
sudo systemctl status wazuh-manager
```

Confirm that Suricata alerts are being processed by Wazuh:

```bash
sudo grep -i suricata /var/ossec/logs/alerts/alerts.json | tail -10
```

Check whether Suricata logs are being monitored:

```bash
sudo grep -a "eve.json" /var/ossec/logs/ossec.log
```

Search for alerts in the Threat Hunting module using:

```text
suricata
```

or

```text
rule.groups:suricata
```

### Resolution

Validated Suricata log generation, Wazuh log ingestion, and dashboard processing until alerts became visible within the Wazuh Threat Hunting Dashboard.

---

## Outcome

After resolving the above issues, the complete detection workflow was successfully validated:

Nmap Scan → Suricata Detection → eve.json Alert Generation → Wazuh Log Ingestion → Wazuh Dashboard Alert
