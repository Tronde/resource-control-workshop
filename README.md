# Resource Control workshop for RHEL 8, 9, and beyond

This repository is part of a resource control workshop. It contains supplementary files and references to documentation.

The story being addressed is to limit resources for applications and services in a way that you are still able to login via SSH and control/administer the system. Or to put it the other way around, to reserve enough resources for smooth systems management.

## Where is the official documentation?

  * [RHEL 9 - Monitoring and managing system status and performance - Chapter 33-35](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/monitoring_and_managing_system_status_and_performance/index)
  * [RHEL 9 - Managing, monitoring, and updating the kernel - Chapter 24 and 25](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_monitoring_and_updating_the_kernel/index)
  * [RHEL 8 - Managing, monitoring, and updating the kernel - Chapter 23-26](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/managing_monitoring_and_updating_the_kernel/index)
  * [Migrating from CGroups V1 in Red Hat Enterprise Linux 7 and below to CGroups V2 in Red Hat Enterprise Linux 8 (login required)](https://access.redhat.com/articles/3735611)

## What other interesting resources are out there?

  * [World domination with cgroups in RHEL 8: welcome cgroups v2!](https://www.redhat.com/en/blog/world-domination-cgroups-rhel-8-welcome-cgroups-v2) by Marc Richter (Principal Technical Account Manager at Red Hat)

## How to exhaust system resources like CPU and Memory?

  * Use a VM with 2 vCPU and 4 GB of memory
  * Run one of the scrips from this repository to stress CPU and/or memory

Here's a systemd service unit that executes stress-ng to use all CPU and virtual memory, but includes cgroup configuration to limit CPU and memory usage to 80% of overall resources.

**Be aware:** Running this service can render your VM unresponsive. Use with caution and on your own risk.

```
[Unit]
Description=Stress-ng CPU and Memory Test with Resource Limits
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/stress-ng --all -1 --maximize --agressive
Restart=on-failure
RestartSec=5s

# CPU limit (80%)
#CPUQuota=80%

# Memory limit (80%)
#MemoryLimit=80%

# Enable CPU and memory accounting
#CPUAccounting=true
#MemoryAccounting=true

[Install]
WantedBy=multi-user.target
```
