# Enabling Network on Ubuntu Server (VirtualBox)

This guide explains how to enable networking on **Ubuntu Server** running in **VirtualBox** using a Host-Only adapter.  
It is ideal for Kubernetes, Ansible, Docker labs, or multi-VM setups.

---

## 📦 Overview

To allow VMs to communicate with each other:

- Use **2 network adapters**
- Enable the **Host-Only adapter**
- Bring the interface up
- Request an IP via DHCP

---

## 1. Configure VirtualBox Network

Open VirtualBox → Select VM → **Settings → Network**

### Adapter 1 — NAT
Provides internet access.

```

Attached to: NAT
Cable Connected: ✔

```

---

### Adapter 2 — Host-only
Provides local VM ↔ VM communication.

```

Attached to: Host-only Adapter
Name: VirtualBox Host-Only Ethernet Adapter
Cable Connected: ✔

````

> If no Host-Only adapter exists:
> VirtualBox → Tools → **Host Network Manager → Create**

---

## 2. Check Network Interfaces (Ubuntu Server)

Boot the VM and run:

```bash
ip addr
````

You should see something like:

* `lo` — loopback
* `enp0s3` — NAT
* `enp0s8` — Host-only

Example output:

```
enp0s8: <NO CARRIER> state DOWN
```

This means the adapter is detected but inactive.

---

## 3. Enable Host-Only Interface

```bash
sudo ip link set enp0s8 up
```

Verify again:

```bash
ip addr
```

---

## 4. Install DHCP Client

Most Ubuntu Server minimal images **do not include a DHCP client**.

Install:

```bash
sudo apt update
sudo apt install -y isc-dhcp-client
```

---

## 5. Request IP Address

```bash
sudo dhclient enp0s8
```

---

## 6. Verify Assigned IP

Check:

```bash
ip addr
```

Expected:

```
enp0s8: inet 192.168.56.101/24
```

> Any IP in the `192.168.56.x` range means success.

---

## 7. Test VM-to-VM Connectivity

On one VM:

```bash
ping 192.168.56.102
```

On the other VM:

```bash
ping 192.168.56.101
```

Both should respond.

---

# Troubleshooting

---

### ❗ enp0s8 not visible

VirtualBox Adapter 2 not enabled.

Check:

```
Settings → Network → Adapter 2 → Host-only Adapter
```

---

### ❗ enp0s8 is UP but no IP

DHCP may be disabled or blocked.

Assign static IP:

```bash
sudo ip addr add 192.168.56.50/24 dev enp0s8
sudo ip link set enp0s8 up
```

---

### ❗ DHCP Disabled in VirtualBox

Go to:

```
VirtualBox → Tools → Host Network Manager
```

Select **VirtualBox Host-Only Ethernet Adapter**

Ensure:

```
DHCP Server: ✔ Enabled
```

---

# Final Network Layout

For multi-VM labs:

* **Adapter 1 (NAT)** → Internet → `10.0.2.x`
* **Adapter 2 (Host-only)** → LAN VM ↔ VM → `192.168.56.x`

Example:

```
Master: enp0s8 = 192.168.56.101
Worker: enp0s8 = 192.168.56.102
```

Perfect for:

* Kubernetes kubeadm clusters
* Multi-node Docker Swarm
* Ansible automation
* CTF or lab environments

---

# Notes

* Always keep Adapter 1 = NAT
* Use Host-only for internal communication
* Do NOT use 10.0.2.x for cluster communication
* Always use 192.168.56.x addresses

---

## 👍 You're ready!
Your Ubuntu Server can now properly communicate with other VMs and access the internet simultaneously.
