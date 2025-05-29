# Disk-Space-On-Kali

Free Up Disk Space on Kali Linux
Enable Remote Docker API from Windows Host

---

## Freeing Up Disk Space on Kali

### 1. Check Disk Usage

**Show disk usage:**

```bash
df -h
```

**Find which folders use the most space:**

```bash
du -h --max-depth=1 / | sort -hr | head -20
```

---

### 2. Remove Unused Packages

**Clean up package leftovers:**

```bash
sudo apt autoremove -y  
sudo apt autoclean -y  
sudo apt clean -y
```

**Find large packages:**

```bash
dpkg-query -Wf '${Installed-Size;10}\t${Package}\n' | sort -rn | head -20
```

**Remove anything you don’t need:**

```bash
sudo apt remove --purge package-name -y
```

Replace `package-name` with the actual name.

---

### 3. Clear System Logs

**Shrink journal logs to 100MB:**

```bash
sudo journalctl --vacuum-size=100M
```

**Delete old logs:**

```bash
sudo rm -rf /var/log/*.log /var/log/*/*.log
```

---

### 4. Clear APT Cache

**Remove all cached packages:**

```bash
sudo apt clean
```

**Check cache size:**

```bash
du -sh /var/cache/apt
```

---

### 5. Remove Large Files

**Search for files over 500MB:**

```bash
sudo find / -type f -size +500M -exec ls -lh {} +
```

**Delete files you don’t need:**

```bash
sudo rm -rf /path/to/file
```

---

### 6. Remove Old Kernels (Advanced)

**List installed kernels:**

```bash
dpkg --list | grep linux-image
```

**Remove older versions:**

```bash
sudo apt remove --purge linux-image-X.X.X-X-generic
```

Keep the latest one only.

---

### 7. Clean Docker

If you use Docker:

```bash
docker system prune -a
```

*Are you checking space often, or just when things break?*

---

## Enable Remote Docker API on Windows 10

Access Docker on your Windows host from Kali over the network.

### 1. Change Docker Settings

* Open Docker Desktop on Windows
* Go to **Settings**
* Under **General**, check:
  `Expose daemon on tcp://localhost:2375 without TLS`
* Apply and restart Docker

Now your Kali machine can talk to Docker on Windows.

---

**Want to test it?**
From Kali, run:

```bash
docker -H tcp://<windows_ip>:2375 info
```

Replace `<windows_ip>` with your actual host IP.
