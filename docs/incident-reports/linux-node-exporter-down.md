# Incident Report: Linux Node Exporter Down

## Summary

A controlled failure test was performed on `linux-01` to verify the monitoring and alerting pipeline.

## Environment

- Host: `linux-01`
- IP: `192.168.157.14`
- Exporter: Node Exporter
- Exporter port: `9100`
- Monitoring server: `monitoring-01`
- Alert: `InstanceDown`
- Notification: Telegram

## Trigger

Node Exporter was intentionally stopped:

```bash
sudo systemctl stop node_exporter
Detection
Prometheus detected the target as unavailable:
up = 0

The alert lifecycle was:
inactive -> pending -> firing

The for: 1m setting prevented the alert from firing immediately.
Alerting
Prometheus successfully sent the alert to Alertmanager.
Alertmanager reported the alert as active and delivered the notification through Telegram.
Recovery
Node Exporter was started again:
sudo systemctl start node_exporter

Prometheus returned:
up = 1

The alert transitioned to:
inactive

Alertmanager then removed the active alert.
A resolved notification was enabled with:
send_resolved: true

Root Cause
No production root cause existed. The service was intentionally stopped as part of a controlled failure test.
Lessons Learned
The test verified the complete monitoring workflow:
Exporter failure
      ↓
Prometheus detection
      ↓
Alert rule evaluation
      ↓
Alertmanager
      ↓
Telegram notification
      ↓
Service recovery
      ↓
Alert resolution

Status
Resolved successfully.
