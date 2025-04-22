## Summary

This guide walks you through adding swap space on an Ubuntu 20.04 server by creating and enabling a swap file, making it persistent across reboots, and tuning kernel swap parameters for optimal performance. You’ll learn how to check existing swap and disk space, create and secure a swap file, enable it immediately, ensure it’s activated on startup, and adjust swappiness and cache-pressure settings.

## Introduction

One way to guard against out‑of‑memory errors in applications is to add swap space to your server. In this guide, you will create a swap file on Ubuntu 20.04, enable it, make it permanent across reboots, and tune swap behavior for your workload.

## What is Swap?

Swap is a portion of disk space set aside for the operating system to store data that it can’t keep in RAM. While accessing swap is much slower than RAM, it provides a safety net against out‑of‑memory exceptions when physical RAM is fully utilized.

## Step 1 – Checking the System for Swap Information

1. Verify whether swap is already active:
   ```bash
   sudo swapon --show
   ```
   If no output is returned, no swap is configured.

2. Confirm with the `free` utility:
   ```bash
   free -h
   ```
   Look at the “Swap” row to see current swap usage.

## Step 2 – Checking Available Space on the Hard Drive Partition

Before creating a swap file, ensure you have sufficient disk space:

```bash
df -h
```

Identify the filesystem mounted on `/` and verify the `Avail` column shows enough free space for your desired swap size.

## Step 3 – Creating a Swap File

1. Create a swap file named `/swapfile` using `fallocate` (adjust `1G` to match your RAM or requirements):
   ```bash
   sudo fallocate -l 1G /swapfile
   ```
2. Verify the file size:
   ```bash
   ls -lh /swapfile
   ```
   You should see a file of the specified size.

## Step 4 – Enabling the Swap File

1. Secure the swap file so only root can read/write it:
   ```bash
   sudo chmod 600 /swapfile
   ```
2. Mark the file as swap space:
   ```bash
   sudo mkswap /swapfile
   ```
3. Enable the swap for the current session:
   ```bash
   sudo swapon /swapfile
   ```
4. Verify activation:
   ```bash
   sudo swapon --show
   free -h
   ```
   The swap row should now reflect your swapfile.

## Step 5 – Making the Swap File Permanent

1. Back up your fstab:
   ```bash
   sudo cp /etc/fstab /etc/fstab.bak
   ```
2. Add swap entry to `/etc/fstab`:
   ```bash
   echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
   ```
   This ensures the swap file is enabled on boot.

## Step 6 – Tuning Your Swap Settings

### Adjusting the Swappiness Property

- **Check current swappiness**:
  ```bash
  cat /proc/sys/vm/swappiness
  ```
  Default is usually `60`.
- **Temporarily set swappiness to 10**:
  ```bash
  sudo sysctl vm.swappiness=10
  ```
- **Persist swappiness across reboots**:
  ```bash
  sudo sh -c 'echo "vm.swappiness=10" >> /etc/sysctl.conf'
  ```

### Adjusting the Cache Pressure Setting

- **Check current vfs_cache_pressure**:
  ```bash
  cat /proc/sys/vm/vfs_cache_pressure
  ```
  Default is `100`.
- **Temporarily set cache pressure to 50**:
  ```bash
  sudo sysctl vm.vfs_cache_pressure=50
  ```
- **Persist cache pressure across reboots**:
  ```bash
  sudo sh -c 'echo "vm.vfs_cache_pressure=50" >> /etc/sysctl.conf'
  ```

## Conclusion

You have successfully added swap space to your Ubuntu 20.04 server, made it persistent, and tuned kernel parameters to control swap usage and cache behavior. Proper swap configuration can help prevent out‑of‑memory errors, but consider upgrading RAM or optimizing applications for long‑term performance.

