# Basic Network Scanning with Nmap

## Overview

This project demonstrates basic network reconnaissance using Nmap in a controlled Kali Linux virtual machine environment.

The objective was to identify open TCP ports, determine the services running on those ports, detect service versions, and attempt operating system identification.

## Objectives

- Perform a basic TCP port scan.
- Identify open ports and associated services.
- Perform service and version detection.
- Attempt operating system detection.
- Save and document Nmap scan results.

## Lab Environment

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Virtualization | Oracle VirtualBox |
| Scanner | Nmap 7.99 |
| Target | Localhost (127.0.0.1) |
| Network Mode | NAT |
| Target Service | OpenSSH |
| Open Port | TCP 22 |

## Scans Performed

### 1. Basic Port Scan

Command:

```bash
nmap 127.0.0.1
