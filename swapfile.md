```
# Swap File Management

This guide outlines the steps to create and configure a swap file on a Linux system.
```

### 1. Check current swap
First, check the current swap usage:
```bash
swapon --show
```

### 2. Disable the current swap
Turn off the current swap file (if any):
```bash
sudo swapoff /swapfile
```

### 3. Create a new swap file
Create a new swap file of 1GB size (1024MB):
```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=1024 oflag=append conv=notrunc
```

### 4. Set up the swap file
Set up the swap file:
```bash
sudo mkswap /swapfile
```

### 5. Enable the swap file
Enable the swap file:
```bash
sudo swapon /swapfile
```

### 6. Verify the swap
Verify that the swap is active:
```bash
swapon --show
```

### Notes:
- Ensure that you have sufficient disk space for the swap file.
- Adjust the `count` parameter in the `dd` command to change the swap size.
