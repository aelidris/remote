# 01_remote: Remote Server Management & Hardening

## 1. Project Overview
This project focuses on managing a virtual machine (Guest) from a host machine (Ubuntu) using **SSH**. The goal is to establish a secure remote connection, change default listening ports, and configure a firewall.

---

## 2. Requirements & Constraints
* **Virtualization:** VirtualBox with the `01_remote` image imported.
* **No Console Access:** The VM display is intentionally disabled; all interaction must happen via terminal.
* **File Protection:** The configuration file `/etc/ssh/sshd_config` is immutable, meaning direct edits are blocked even for the root user.

---

## 3. Implementation Steps

### Phase 1: Initial Connection
Because the VM is behind a NAT router, you must map a host port to the guest port.
1. Go to **Settings > Network > Advanced > Port Forwarding**.
2. Add a new rule:
   - **Name:** `SSH_Initial`
   - **Protocol:** `TCP`
   - **Host Port:** `4242`
   - **Guest Port:** `22` (Default SSH)



Connect from your Ubuntu terminal:
```bash
ssh -p 4242 root@localhost