# Runbook: Instance Down

## Alert

`InstanceDown`

## Symptoms

Prometheus reports:

```promql
up == 0
he target is unreachable for longer than the configured alert duration.
Impact
Prometheus cannot collect metrics from the affected target.
Investigation
1. Check Prometheus target
On monitoring-01:
curl -s http://localhost:9090/api/v1/targets

Check:
- target health
- lastError
- target address
2. Test exporter endpoint
Linux:
curl http://<linux-ip>:9100/metrics

Windows:
curl http://<windows-ip>:9182/metrics

3. Check exporter service
Linux:
sudo systemctl status node_exporter

Windows PowerShell:
Get-Service windows_exporter

4. Check firewall and network
Linux:
sudo ufw status

Verify TCP/9100 is reachable from monitoring-01.
Windows:
Get-NetFirewallRule

Verify TCP/9182 is reachable from monitoring-01.
Recovery
Linux:
sudo systemctl start node_exporter

Windows:
Start-Service windows_exporter

Verify Prometheus reports:
up{job="linux"} == 1

or:
up{job="windows"} == 1

Escalation
If the exporter is running but the target remains down:
1. Check network connectivity.
2. Check firewall rules.
3. Check exporter logs.
4. Check Prometheus lastError.
5. Verify target IP and exporter port.
