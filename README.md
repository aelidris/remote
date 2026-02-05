# 01_remote: SSH Management & System Hardening

## 1. Project Concept

The goal of this project is to manage a virtualized server via a **distant shell**. In a real-world server environment, physical access is unavailable; therefore, we use **SSH (Secure Shell)** to interact with the system.

### Why use SSH?

- **Comfort:** Allows the use of your native terminal on the host machine.
- **Customization:** Retains your personal terminal themes, fonts, and keyboard maps.
- **Productivity:** Enables features like copy-pasting and multiple terminal windows that are often unavailable in raw virtual consoles.

---

## 2. Infrastructure: The NAT Bridge

Because the VM is behind a virtual router, it is not directly accessible from your host. We must bridge the connection using **Port Forwarding**:

- **Host:** Refers to your physical Ubuntu machine.
- **Guest:** Refers to the VirtualBox VM.
- **Rule:** We map a port on our machine (Host Port `4242`) to a port on the VM (Guest Port).

---

## 3. Implementation Steps

### Phase 1: Initial Remote Connection

1. Import the `01_remote` image into VirtualBox.
2. Go to **Settings > Network > Advanced > Port Forwarding**.
3. Create a rule: **Host Port `4242`** → **Guest Port `22`** (SSH default).
4. Connect via your host terminal:

```bash
ssh -p 4242 root@localhost
```

> Note: This is the only way to interact as consoles are disabled.

---

### Phase 2: System Hardening

Changing the default port (`22`) is a standard security practice to prevent automated bots from attempting brute-force logins.

#### Change SSH Port

Navigate to:

```
/etc/ssh/sshd_config
```

Change the port from default port "22" to anotherone:

```bash
Port 2222
```

#### Update Firewall (UFW)

The firewall must be instructed to allow the new traffic:

```bash
ufw allow 2222/tcp
ufw delete allow 22/tcp  # Remove old port for security
ufw enable
```

#### Restart Service

Apply changes:

```bash
systemctl restart ssh
```

---

### Phase 3: Synchronizing Port Forwarding

After changing the internal port to `2222`, the VirtualBox rule must be updated:

1. Return to **VirtualBox Settings > Port Forwarding**.
2. Change the **Guest Port** from `22` to `2222`.

---

## 4. Final Verification Commands

| Command | Purpose |
|--------|---------|
| `ufw status` | Confirms only port `2222/tcp` is allowed |
| `ss -tunlp \| grep ssh` | Confirms the SSH service is listening on port `2222` |
| `ssh -p 4242 root@localhost` | Final test of the remote connection |

---

Created for the **01_remote evaluation**.

You can now run:

```bash
ufw delete allow 22/tcp
```

to finalize the security.
