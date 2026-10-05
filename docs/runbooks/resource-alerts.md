# Runbook: Resource Alerts

## Alerts

Linux:

- `LinuxHighCPU`
- `LinuxHighMemory`
- `LinuxLowDisk`

Windows:

- `WindowsHighCPU`
- `WindowsHighMemory`
- `WindowsLowDisk`

## 1. High CPU

### Linux

```promql
100 - (
  avg by(instance) (
    rate(node_cpu_seconds_total{
      job="linux",
      mode="idle"
    }[5m])
  ) * 100
)
Windows
100 - (
  avg by(instance) (
    rate(windows_cpu_time_total{
      job="windows",
      mode="idle"
    }[5m])
  ) * 100
)

Investigation
1. Identify the affected host.
2. Check which process is consuming CPU.
3. Determine whether the load is expected.
4. Check recent service or scheduled-task changes.
5. Take corrective action only after identifying the cause.
2. High Memory
Linux
100 * (
  1 - (
    node_memory_MemAvailable_bytes{job="linux"}
    /
    node_memory_MemTotal_bytes{job="linux"}
  )
)

Windows
100 * (
  1 - (
    windows_memory_physical_free_bytes{job="windows"}
    /
    windows_memory_physical_total_bytes{job="windows"}
  )
)

Investigation
1. Identify the affected host.
2. Check memory-intensive processes.
3. Look for abnormal memory growth.
4. Check whether a service is leaking memory.
5. Determine whether additional capacity is required.
3. Low Disk
Linux
100 * (
  node_filesystem_avail_bytes{job="linux"}
  /
  node_filesystem_size_bytes{job="linux"}
)

Windows
100 * (
  windows_logical_disk_free_bytes{job="windows"}
  /
  windows_logical_disk_size_bytes{job="windows"}
)

Investigation
1. Identify the affected filesystem or drive.
2. Check large files and logs.
3. Remove only known-safe temporary data.
4. Check application logs for abnormal growth.
5. Consider increasing disk capacity if required.
Recovery Verification
After corrective action:
1. Verify the metric has returned to a normal range.
2. Confirm the Prometheus alert becomes inactive.
3. Confirm Alertmanager no longer has the alert as active.
4. Confirm Telegram receives the resolved notification.
Alert Thresholds
Alert	Threshold	Duration
High CPU	> 85%	5m
High Memory	> 90%	5m
Low Disk	< 15% free	10m
