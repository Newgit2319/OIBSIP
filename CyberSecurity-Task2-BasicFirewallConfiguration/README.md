# CyberSecurity Task 2 – Basic Firewall Configuration

## Objective

Configure and verify a basic firewall using UFW (Uncomplicated Firewall) on Kali Linux.

## Lab Environment

| Component | Details |
|---|---|
| Operating System | Kali Linux |
| Firewall | UFW |
| Target System | Local Kali Linux VM |
| Firewall Policy | Deny incoming, allow outgoing |

## Firewall Configuration

The following default policies were configured:

- Incoming connections: DENY
- Outgoing connections: ALLOW

## Allowed Service

SSH traffic was explicitly allowed on TCP port 22.

```bash
sudo ufw allow ssh
## Allowed Service

SSH traffic was explicitly allowed on TCP port 22.

sudo ufw allow ssh

Result:

22/tcp ALLOW IN Anywhere
22/tcp (v6) ALLOW IN Anywhere (v6)

## Blocked Service

Telnet traffic was explicitly denied on TCP port 23.

sudo ufw deny 23/tcp

Result:

23/tcp DENY IN Anywhere
23/tcp (v6) DENY IN Anywhere (v6)

## Firewall Logging

UFW logging was enabled:

sudo ufw logging on

Logging status:

Logging: on (low)

## Verification

The final firewall status was verified using:

sudo ufw status verbose
sudo ufw status numbered

Final configuration:

- Firewall status: Active
- Incoming policy: Deny
- Outgoing policy: Allow
- SSH (TCP 22): Allowed
- Telnet (TCP 23): Denied
- Logging: Enabled

## Evidence

The evidence/ directory contains:

- firewall_rules.txt - numbered firewall rules
- firewall_status.txt - detailed firewall status
- firewall_added_rules.txt - configured UFW rules

The screenshots/ directory contains visual evidence of the firewall configuration.

## Conclusion

A basic host-based firewall was successfully configured using UFW on Kali Linux. Incoming connections are denied by default, SSH access is explicitly permitted, and Telnet traffic is explicitly blocked. Firewall logging was also enabled and the configuration was verified successfully.

