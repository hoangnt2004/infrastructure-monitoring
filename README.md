# Infrastructure Monitoring System

A centralized monitoring system for Linux and Windows servers.

## Project Overview

This project builds a monitoring environment for server infrastructure,
using Prometheus, Grafana and Alertmanager to collect metrics,
visualize system performance and handle alerts.

## Scope

The system monitors:

- Linux Server
- Windows Server

Main monitoring metrics:

- CPU
- Memory
- Disk / Filesystem
- Network
- Uptime
- Service status

## Architecture

```text
Linux Server
    │
    └── Node Exporter
             │
             │
Windows Server
    │
    └── Windows Exporter
             │
             ▼
        Prometheus
          │     │
          │     └──────────► Alertmanager ──► Telegram
          │
          ▼
        Grafana
```

Technology Stack

- Prometheus
- Node Exporter
- Windows Exporter
- Grafana
- Alertmanager
- Docker Compose
- Bash
- PowerShell
- Python
  Project Goals
- Build centralized server monitoring.
- Create reusable Grafana dashboards.
- Design Prometheus alert rules.
- Configure Alertmanager notification workflows.
- Practice troubleshooting and incident investigation.
- Automate common operational tasks.
- Document the deployment and troubleshooting process.
  Project Status
  Work in progress.
  This repository is being developed step by step while learning
  and implementing the monitoring system.
