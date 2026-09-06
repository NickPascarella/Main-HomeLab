# Linux Administration

[← Back to Main Portfolio](../README.md)

## Overview

Main HomeLab includes multiple Linux systems used to practice enterprise-style server administration across both on-premises and AWS environments.

The environment combines Rocky Linux systems in the on-premises Servers VLAN with Amazon Linux EC2 instances in AWS. Administration includes operating-system management, service control, networking, SSH, permissions, storage, package maintenance, patching, troubleshooting, and operational validation.

The objective was to manage Linux as part of an integrated infrastructure environment rather than as isolated virtual machines.

## Linux Systems

| System | Platform | Network / Address | Primary Role |
|---|---|---|---|
| server01 | Rocky Linux 10 | 10.20.20.10 | Infrastructure services, Ansible control, monitoring, logging, NFS |
| server02 | Rocky Linux 10 | 10.20.20.20 | Application/server workloads, NFS client, backup operations |
| aws-server01 | Amazon Linux 2023 | 10.100.10.132 | Public AWS administration/bastion host |
| aws-server02 | Amazon Linux 2023 | 10.100.20.212 | Private AWS Linux server |

The on-premises Linux servers reside in VLAN 20 (`10.20.20.0/24`), while the AWS systems reside in separate public and private VPC subnets.

## Multi-Distribution Administration

The lab provides administration experience across two Linux distributions:

- Rocky Linux 10
- Amazon Linux 2023

This required working with differences in cloud and on-premises deployment while applying common Linux administration concepts such as:

- systemd service management
- package administration
- filesystem management
- SSH
- permissions
- networking
- process and port validation
- log inspection
- patching
- troubleshooting

## Service Management with systemd

Linux services are managed and validated using systemd.

Typical administrative commands used throughout the project include:

```bash
systemctl status <service>
systemctl start <service>
systemctl restart <service>
systemctl enable <service>
systemctl is-active <service>
systemctl is-enabled <service>
journalctl -u <service>
```

This approach was used to manage infrastructure services including monitoring agents, logging components, backup timers, NFS services, and other Linux workloads.

Troubleshooting focused on validating both the service state and the actual application behavior rather than assuming that an `active` systemd unit automatically meant a service was functioning correctly.

## Network Administration

The on-premises Linux servers use the Servers VLAN:

```text
VLAN 20 — SERVERS
Network: 10.20.20.0/24
Gateway: 10.20.20.1

server01: 10.20.20.10
server02: 10.20.20.20
```

Linux network troubleshooting included validating:

- IP addressing
- routing
- DNS resolution
- interface state
- listening TCP/UDP ports
- host firewall configuration
- remote TCP connectivity
- application-layer responses

Tools used during troubleshooting included:

```bash
ip addr
ip route
ss
ping
curl
nc
firewall-cmd
```

This layered approach helped distinguish application problems from host-firewall, routing, switching, and browser/client issues.

## SSH Administration

SSH provides the primary remote administration method for Linux systems.

The project included:

- SSH key-based authentication
- administrative access between Linux systems
- disabling SSH password authentication in AWS
- dedicated Ansible SSH keys
- controlled access to private AWS systems through a public bastion host

The private AWS instance does not have a public IPv4 address. Administration reaches it through `aws-server01` using SSH ProxyCommand rather than exposing the private system directly to the Internet.

Detailed AWS access architecture is documented separately in the AWS Hybrid Cloud section.

## Package and Patch Management

Package administration and patching were incorporated into routine Linux operations.

Administrative tasks included:

- package installation
- package updates
- security and maintenance patching
- validation after updates
- service-state verification
- reboot-awareness
- repeatable patch-management procedures

Patch management was eventually automated and validated across managed Linux hosts while preserving the ability to troubleshoot individual systems manually.

## Filesystems and Storage

Linux storage administration includes both local filesystems and centralized NFS storage.

`server01` exports centralized storage that is mounted by `server02`:

```text
server01:/srv/nfs/shared
        │
        │ NFSv4
        ▼
server02:/mnt/shared
```

Administrative work included:

- filesystem mounting
- persistent storage configuration
- permissions
- capacity validation
- NFS troubleshooting
- backup storage
- filesystem usage monitoring

Commands such as the following were used to validate storage:

```bash
findmnt
df -h
mount
ls -l
```

Detailed backup and recovery implementation is documented separately in the Backup & Recovery section.

## Permissions and Security

Linux administration throughout the project required attention to permissions and service security.

Examples include:

- file and directory ownership
- service-account permissions
- SSH key permissions
- sudo/root privilege requirements
- NFS permission behavior
- SELinux-related filesystem behavior
- service-specific access requirements

Several project troubleshooting scenarios resulted from permissions rather than network connectivity, reinforcing the importance of validating ownership, mode bits, security context, and the identity under which a service executes.

## Operational Validation

Linux administration tasks were validated using both direct shell commands and centralized Ansible execution.

Validation included:

- hostname and operating-system identification
- kernel versions
- system uptime
- service state
- listening ports
- filesystem utilization
- network reachability
- application HTTP responses
- patch state
- backup status

This provided a consistent way to verify that configuration changes resulted in the expected operational state.

## Implementation Evidence

### Hybrid Linux Administration

The portfolio includes validation across the complete Linux environment:

```text
aws-server01 — Amazon Linux 2023
aws-server02 — Amazon Linux 2023
server01      — Rocky Linux 10
server02      — Rocky Linux 10
```

Evidence collected from the managed systems validates operating-system versions, kernels, hostnames, uptime, and remote administrative access.

> Screenshot evidence will be added here from the completed Main HomeLab validation set.

## Troubleshooting Case Study — Service Reachability

### Problem

During Grafana validation, the service initially appeared unreachable from the Windows management workstation.

### Investigation

Troubleshooting proceeded through the infrastructure stack rather than immediately changing configuration.

Validation confirmed:

- `grafana-server` was active
- Grafana was listening on TCP port 3000
- local HTTP requests returned a valid response
- firewalld permitted TCP/3000
- the Windows workstation could ping `server01`
- a TCP connection from the management workstation to `10.20.20.10:3000` succeeded

The server-side application and network path were therefore operational.

### Resolution

No infrastructure configuration change was required. Subsequent browser access succeeded, indicating the earlier timeout was transient or client/browser related.

### Lesson Learned

A failed browser connection does not automatically indicate a failed server application or network.

Troubleshooting should validate each layer independently:

```text
Service
  ↓
Listening socket
  ↓
Local application response
  ↓
Host firewall
  ↓
IP connectivity
  ↓
Remote TCP connectivity
  ↓
Client application
```

This prevents unnecessary configuration changes to infrastructure that is already functioning correctly.

## Skills Demonstrated

- Rocky Linux administration
- Amazon Linux administration
- systemd service management
- journal and log analysis
- SSH administration
- SSH key authentication
- Linux networking
- TCP/UDP port validation
- firewalld administration
- package management
- patch management
- filesystem administration
- NFS client/server operations
- permissions and ownership
- service troubleshooting
- multi-host Linux administration
- hybrid on-premises/cloud administration
- operational validation and documentation

---

[← Back to Main Portfolio](../README.md)
