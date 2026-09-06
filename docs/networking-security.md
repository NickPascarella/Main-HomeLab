# Networking & Security

[← Back to Main Portfolio](../README.md)

## Overview

Main HomeLab uses an enterprise-style segmented network built around a Cisco Catalyst 3850 Layer-3 core switch, Cisco Catalyst 3750 access switch, and pfSense firewall/router.

The network was designed to provide separate security and functional zones for management, servers, clients, storage, DMZ systems, and container workloads while maintaining controlled communication between those networks.

## Network Architecture

| Component | Role |
|---|---|
| Cisco Catalyst 3850 | Layer-3 core switching, inter-VLAN routing, SVIs, and ACL enforcement |
| Cisco Catalyst 3750 | Layer-2 access switching and endpoint connectivity |
| pfSense | Upstream routing, firewall services, and Internet connectivity |
| Raspberry Pi 5 | DMZ host (`dmz01`) |
| server01 / server02 | Linux systems on the Servers VLAN |
| Management workstation | Administrative system on the Management VLAN |

## VLAN Design

| VLAN | Name | Network | Gateway |
|---|---|---|---|
| 10 | MANAGEMENT | 10.10.10.0/24 | 10.10.10.1 |
| 20 | SERVERS | 10.20.20.0/24 | 10.20.20.1 |
| 30 | CLIENTS | 10.30.30.0/24 | 10.30.30.1 |
| 40 | STORAGE | 10.40.40.0/24 | 10.40.40.1 |
| 50 | DMZ | 10.50.50.0/24 | 10.50.50.1 |
| 60 | CONTAINERS | 10.60.60.0/24 | 10.60.60.1 |
| 99 | NATIVE | — | — |

The Cisco 3850 provides the Layer-3 gateway for each routed VLAN.

## Core-to-Access Switching

The operational 802.1Q trunk between the switches is:

```text
CoreSwitch Gi1/0/47
        │
        │ 802.1Q trunk
        │ Native VLAN 99
        │ Allowed VLANs:
        │ 10,20,30,40,50,60,99
        │
AccessSwitch Fa1/0/48
