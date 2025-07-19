# 🧹 Kali Disk Cleanup & Remote Docker Access  
Free space like a pro, then steer Docker on your Windows host—all from your Kali box.

---

## 🔖 Repo‑name ideas  
* `kali-disk-cleanup-docker`  
* `kali-space-saver-remote-docker`  
* `daph-kali-tidy-lab`  
* `free-up-kali-disk`  

*(pick the kebab‑case label that fits your portfolio style)*

---

## 🙋‍♀️ About Me (blurb for README header)  
*“I automate the un‑fun bits—cleaning disks, wiring APIs—so I can spend more time hacking and less time hunting gigabytes.”*

---

## 🗂️ Contents  
| Section | What you get |
|---------|--------------|
| **Disk Cleanup Cheatsheet** | One‑liners to reclaim GBs fast |
| **Docker Remote API How‑To** | Point Kali’s CLI at Docker Desktop on Windows |
| **Safety Tips** | Avoid nuking the wrong files |
| **Next Steps** | Ideas to script, monitor, and automate |

---

## 🧼 Freeing Up Disk Space on Kali

| Step | Command(s) | Why |
|------|------------|-----|
| **1. Check usage** | `df -h`<br>`du -h --max-depth=1 / | sort -hr | head -20` | Spot the space hogs |
| **2. Purge orphan pkgs** | `sudo apt autoremove -y`<br>`sudo apt autoclean -y`<br>`sudo apt clean -y` | Yank leftovers & cache |
| **3. Nuke large pkgs** | `dpkg-query -Wf '${Installed-Size;10}\t${Package}\n' \| sort -rn \| head -20`<br>`sudo apt remove --purge <package>` | Remove heavy, unused apps |
| **4. Trim logs** | `sudo journalctl --vacuum-size=100M`<br>`sudo rm -rf /var/log/*.log /var/log/*/*.log` | Logs grow fast—cap or clear |
| **5. Clear APT cache** | `sudo apt clean`<br>`du -sh /var/cache/apt` | Cache ≠ permanent storage |
| **6. Hunt giant files** | `sudo find / -type f -size +500M -exec ls -lh {} +`<br>`sudo rm -rf /path/to/file` | Delete ISO duplicates, etc. |
| **7. Remove old kernels (⚠ advanced)** | `dpkg --list \| grep linux-image`<br>`sudo apt remove --purge linux-image-X.X.X-X-generic` | Keep the newest only |
| **8. Docker detox** | `docker system prune -a` | Containers & images eat GBs |

> **Pro tip:** log every deletion in `cleanup-log.md`—future you will thank you.

---

## 🐳 Enable Remote Docker API (Windows → Kali)

1. **On Windows (Docker Desktop)**  
   * Settings ▸ **General** ▸ ✔ *Expose daemon on tcp://localhost:2375*  
   * Apply & Restart

2. **On Kali**  
   ```bash
   windows_ip="<WinHost_IP>"
   docker -H tcp://$windows_ip:2375 info
   ```
   You should see Docker info from the Windows engine.

3. **Firewall notes**  
   *2375 is plaintext—use it only on trusted lab nets or wrap it in an SSH tunnel.*

---

## 🔒 Safety tips

* **Double‑check paths** before any `rm -rf`.  
* Keep **one known‑good snapshot** of the VM before major purges.  
* Test the Docker API from a **non‑privileged user** first.

---

## ➡️ Next steps

* Script all cleanup commands into `cleanup.sh`; schedule via `cron`.  
* Add **Prometheus + Grafana** to graph disk usage over time.  
* Secure Docker API with **TLS or an SSH tunnel** for real deployments.  

Happy tidying—may your disks stay slim and your exploits stay sharp! 🗡️
