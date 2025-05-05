## 💾 Ubuntu 20.04 Swap Space Setup Guide

This friendly, emoji-enhanced guide walks you through setting up and tuning swap space on Ubuntu 20.04. It includes clear steps to check your system, create and secure swap, make it persist on reboot, and tune it for best performance. 🐧⚙️

---

### 📘 What is Swap?

Swap is a section of disk used when your system runs out of RAM. Though much slower than RAM, it acts as an overflow to help prevent crashes and out-of-memory errors. 🧠➡️💽

---

### ✅ Step 1 – Check for Existing Swap

1. 🔍 Check if swap is active:

   ```bash
   sudo swapon --show
   ```

   ➡️ *No output means no swap is currently configured.*

2. 🧮 Use `free -h` to see memory and swap usage:

   ```bash
   free -h
   ```

   ➡️ *Look for the "Swap" row.*

---

### 📦 Step 2 – Check Available Disk Space

Before creating a swap file, make sure your disk has enough space:

```bash
df -h
```

➡️ *Check the `Avail` column for `/` (root partition).* Make sure you have space for your swap file size (e.g., 1G).

---

### 🛠️ Step 3 – Create the Swap File

1. 🗂️ Create a 1GB swap file (adjust size as needed):

   ```bash
   sudo fallocate -l 1G /swapfile
   ```

2. ✅ Verify size:

   ```bash
   ls -lh /swapfile
   ```

---

### 🔐 Step 4 – Enable the Swap File

1. 🔒 Secure the file:

   ```bash
   sudo chmod 600 /swapfile
   ```

2. 🧮 Format it for swap:

   ```bash
   sudo mkswap /swapfile
   ```

3. ⚡ Activate swap:

   ```bash
   sudo swapon /swapfile
   ```

4. 🧪 Verify it:

   ```bash
   sudo swapon --show
   free -h
   ```

---

### ♻️ Step 5 – Make It Permanent

1. 🛡️ Backup fstab:

   ```bash
   sudo cp /etc/fstab /etc/fstab.bak
   ```

2. 📝 Add entry:

   ```bash
   echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
   ```

➡️ *Ensures swap is enabled at every boot.*

---

### 🎛️ Step 6 – Tune Swap Settings

#### 🔄 Swappiness (How aggressively swap is used)

1. 📖 Check current:

   ```bash
   cat /proc/sys/vm/swappiness
   ```

2. ⏱️ Set to 10 (less frequent usage):

   ```bash
   sudo sysctl vm.swappiness=10
   ```

3. 💾 Make persistent:

   ```bash
   echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
   ```

#### 📉 Cache Pressure (inode/dentry reclaim rate)

1. 🔍 Check current:

   ```bash
   cat /proc/sys/vm/vfs_cache_pressure
   ```

2. ⚖️ Set to 50 (more aggressive caching):

   ```bash
   sudo sysctl vm.vfs_cache_pressure=50
   ```

3. 💾 Make persistent:

   ```bash
   echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf
   ```

---

### 🎉 Conclusion

You’ve now:

* ✅ Verified your current swap status
* 📦 Created a secure swap file
* 🔁 Made it persistent on reboot
* 🎛️ Tuned swappiness and cache pressure

🔧 Swap helps handle memory spikes, but upgrading RAM is better for long-term performance. Stay optimized and keep your apps running smoothly! 💻🚀

Would you like a PDF version of this guide? 📄😊
