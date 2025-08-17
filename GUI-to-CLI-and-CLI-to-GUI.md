# Removing GUI and Switching to CLI & Vice Versa in Red Hat Linux and Its Clones

> This guide explains how to **remove the GUI environment** and switch to a pure **CLI (Command Line Interface)** and vice versa in Red Hat systems.

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
```bash
  sudo dnf groupremove "Server with GUI" "Workstation" "GNOME"
```

### 4. Remove Display Manager (Login Screen)

- **For GDM (GNOME Display Manager):**
```bash
  sudo systemctl disable gdm
```
```bash
  sudo dnf remove gdm -y
```
or
```bash
  sudo yum remove gdm -y  
```

- **For LightDM (if installed):**
```bash
  sudo systemctl disable lightdm
```

```bash
  sudo dnf remove lightdm -y
```
or
```bash
  sudo yum remove lightdm -y    
```

- **For SDDM (KDE Display Manager):**
```bash
  sudo systemctl disable sddm
```

```bash
  sudo dnf remove sddm -y
```
or
```bash
  sudo dnf remove sddm -y
```

### 5. Clean Up Dependencies
```bash
  sudo dnf autoremove -y
```
or
```bash
  sudo yum autoremove -y
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

## Steps to Switch from CLI to GUI

### 1. Install GUI Packages
```bash
  sudo dnf groupinstall "Server with GUI" -y
```

For **KDE (Optional)**:
```bash
  sudo dnf groupinstall "KDE Plasma Workspaces" -y
```

---

### 2. Enable Display Manager
- **For GDM (GNOME Display Manager):**
```bash
  sudo systemctl enable gdm
```
```bash
  sudo systemctl set-default graphical.target
```

- **For LightDM (if installed):**
```bash
  sudo systemctl enable lightdm
```
```bash
  sudo systemctl set-default graphical.target
```

- **For SDDM (KDE Display Manager):**
```bash
  sudo systemctl enable sddm
```
```bash
  sudo systemctl set-default graphical.target
```

---

### 3. Start GUI Immediately (Without Reboot)
```bash
  sudo systemctl isolate graphical.target
```

---

### 4. Reboot to Apply Changes
```bash
  sudo reboot
```

---

### 5. Post-Reboot Verification
- Ensure system boots into **graphical login screen**.  
- Verify default target:
```bash
  systemctl get-default
```
Output should be:
```
  graphical.target
```
---

> Note:
- Make sure you have **SSH enabled** before removing GUI, so you can still access your server remotely:
```bash
  sudo systemctl enable sshd
```
```bash
  sudo systemctl start sshd
```

---

- Verify network connectivity after GUI removal:
```bash
  ping -c 4 google.com
```

---
- Recommended before removing GUI: backup important configuration files and confirm console/SSH access.
---

## Supported Linux Distributions

This guide applies primarily to **Red Hat Linux** and its derivatives, but can also be adapted for:

- **RHEL (Red Hat Enterprise Linux) 7/8/9**
- **CentOS 7, 8, 9, 10**
- **Rocky Linux (8/9)**
- **AlmaLinux (8/9)**
- **Oracle Linux (7/8/9)**

---

> Note: Commands may vary slightly depending on package manager (`yum` vs `dnf`) and display manager installed (GDM, LightDM, SDDM). Always verify with your system version.

---

## References
- [CentOS Documentation](https://www.centos.org/docs/)
- [Systemd Targets Documentation](https://www.freedesktop.org/software/systemd/man/systemd.target.html)

