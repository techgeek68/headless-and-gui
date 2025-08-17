# Completely Removing GUI and Switching to CLI in CentOS

This guide explains how to **remove the GUI environment** and switch to a pure **CLI (Command Line Interface)** in CentOS systems.

---

## Steps to Switch from GUI to CLI

### 1. Switch Default Boot Target to CLI (Multi-User Mode)
```bash
sudo systemctl set-default multi-user.target
```
This ensures the system boots into CLI mode on restart.

### 2. Stop the GUI Immediately
```bash
sudo systemctl isolate multi-user.target
```
Kills the GUI session without rebooting (useful for active sessions).

### 3. Remove GUI Packages
For **CentOS 8/9/10**:
```bash
sudo dnf groupremove "Server with GUI" "Workstation" "GNOME"
```

For **CentOS 7**:
```bash
sudo yum groupremove "GNOME Desktop"
```

### 4. Remove Display Manager (Login Screen)
- **For GDM (GNOME Display Manager):**
```bash
sudo systemctl disable gdm
sudo dnf remove gdm -y      # CentOS 8/9/10
sudo yum remove gdm -y      # CentOS 7
```

- **For LightDM (if installed):**
```bash
sudo systemctl disable lightdm
sudo yum remove lightdm -y  # CentOS 7
sudo dnf remove lightdm -y  # CentOS 8/9
```

- **For SDDM (KDE Display Manager):**
```bash
sudo systemctl disable sddm
sudo dnf remove sddm -y
```

### 5. Clean Up Dependencies
```bash
sudo dnf autoremove -y      # CentOS 8/9/10
sudo yum autoremove -y      # CentOS 7 (if available)
```

### 6. Disable GUI Services (Optional)
```bash
sudo systemctl mask graphical.target
```

### 7. Reboot the System
```bash
sudo reboot
```

---

## Post-Reboot Verification

- **Confirm CLI Boot:**
  After reboot, you should see a text-based login prompt (not a graphical screen).

- **Check Default Target:**
```bash
systemctl get-default
```
Output should be:
```
multi-user.target
```

---

## Additional Notes

- To **re-enable GUI later**:
```bash
sudo systemctl set-default graphical.target
sudo systemctl isolate graphical.target
```

- Make sure you have **SSH enabled** before removing GUI, so you can still access your server remotely:
```bash
sudo systemctl enable sshd
sudo systemctl start sshd
```

- Verify network connectivity after GUI removal:
```bash
ping -c 4 google.com
```

- Recommended before removing GUI: backup important configuration files and confirm console/SSH access.

---

## Tested Versions
- CentOS 7 (with GNOME)
- CentOS 8, 9, 10 (with GNOME, KDE, or Workstation)

---

## Supported Linux Distributions

This guide applies primarily to **CentOS** and its derivatives, but can also be adapted for:

- **CentOS 7, 8, 9, 10**
- **RHEL (Red Hat Enterprise Linux) 7/8/9**
- **Rocky Linux (8/9)**
- **AlmaLinux (8/9)**
- **Oracle Linux (7/8/9)**

> ⚠️ Note: Commands may vary slightly depending on package manager (`yum` vs `dnf`) and display manager installed (GDM, LightDM, SDDM). Always verify with your system version.

## References
- [CentOS Documentation](https://www.centos.org/docs/)
- [Systemd Targets Documentation](https://www.freedesktop.org/software/systemd/man/systemd.target.html)

