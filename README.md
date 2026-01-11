_This project has been created as part of the 42 curriculum by sywee._

## 📖 Project Description

**Born2beRoot** is a system administration project that focuses on installing, configuring, and securing a Linux server inside a virtual machine. The goal is to create a **stable, minimal, and secure system** by applying strict rules related to:

- User and group management
- Sudo configuration
- SSH access
- Firewall rules
- Password security policies
- System monitoring and automation

---

## ⚡ Key Concepts:

- **Virtual Machine (VM):** Isolated environment running a computer inside another computer.
- **OS Choice:** Debian (Stable, beginner-friendly, community-oriented).
- **Root User:** Superuser with full privileges; must be used cautiously.
- **Sudo:** Provides temporary root privileges safely.
- **Firewall (UFW):** Controls network traffic; only **port 4242** open for SSH.
- **SSH Config:** Secure remote access; **root login disabled**.
- **LVM:** Encrypted, flexible partitioning.
- **Cron:** Automates monitoring script every 10 minutes.

---

## OS Choice - Debian: 

**Pros**
- Very stable and reliable
- Large and well-documented package repository
- Lightweight and suitable for servers
- Strong community support
- AppArmor enabled by default

**Cons**
- Packages may be older compared to rolling-release distributions
- Less frequent updates for bleeding-edge software

## Required Comparisons
### Debian vs Rocky Linux

| Debian | Rocky Linux |
|------|-------------|
| Community-driven | Enterprise-oriented |
| Very stable | Very stable |
| AppArmor by default | SELinux by default |
| Lightweight | Heavier, enterprise-focused |
| Ideal for learning and servers | Ideal for enterprise environments |

---

### AppArmor vs SELinux

| AppArmor | SELinux |
|--------|---------|
| Profile-based | Label-based |
| Easier to configure | More complex |
| Human-readable rules | Steeper learning curve |
| Enabled by default on Debian | Enabled by default on Rocky |

---

### UFW vs firewalld

| UFW | firewalld |
|----|-----------|
| Simple and user-friendly | More powerful and flexible |
| Rule-based | Zone-based |
| Ideal for beginners | Preferred in enterprise setups |
| Default on Debian | Default on Rocky |

---

### VirtualBox vs UTM

| VirtualBox | UTM |
|-----------|-----|
| Cross-platform | macOS-focused |
| Supports x86 architectures | Supports Apple Silicon (ARM) |
| Widely used at 42 | Required for M1/M2 Macs |
| More configuration options | Easier setup on macOS |

---

## 🛠️ Main Design & Security Choices

  - `apt`: Standard package manager for basic tasks  
  - `aptitude`: Handles complex dependency conflicts more intelligently
  - **AppArmor:** Enabled at startup to restrict program capabilities.
  - **Sudoers.d:** Organizes sudo rules without breaking `/etc/sudoers`.
  - **Sudo Policy**: Limited to 3 attempts, custom error message, and full input/output logging in /var/log/sudo/.

### **Password Policy (PAM):** Enforces:
  - Minimum Length: Your password must be at least 10 characters long.
  - Character Types: It must contain at least one uppercase letter, one lowercase letter, and one number.
  - Repetition: It must not contain more than 3 consecutive identical characters.
  - Username Restriction: The password cannot include the name of the user.
  - History Check: For all users except root, the new password must have at least 7 characters that were not part of the former password.
  - Expiration & Aging Rules
  - Max Age: Passwords must expire every 30 days.
  - Min Age: Users must wait at least 2 days before they are allowed to modify a password again.
  - Warning: Users must receive a warning message 7 days before their password expires.

---

### Disk Partitioning
- The system uses **LVM** with at least **two encrypted partitions**.

---

### User and Group Management
- A user with my login was created.
- The user belongs to the following groups:
  - `user42`
  - `sudo`

---

### Signature
The **SHA1 checksum** of the virtual disk file (`.vdi` or `.qcow2`) was generated and saved in a file named `signature.txt` at the **root of the repository**. 

---

### 📊 Monitoring Script (`monitoring.sh`)

A Bash script that broadcasts system telemetry to all terminals every 10 minutes.

| Component       | Command                  | Description |
|-----------------|-------------------------|-------------|
| OS & Kernel     | `uname -a`              | Shows OS and kernel version |
| CPU             | `grep /proc/cpuinfo`    | Counts sockets and cores |
| RAM Usage       | `free --mega`           | Shows used/total RAM and percentage |
| Disk Usage      | `df -m`                 | Shows disk usage (excludes `/boot`) |
| CPU Load        | `vmstat`                | 100% minus idle percentage |
| Network         | `hostname -I` & `ip link` | Shows IP and MAC addresses |
| Sudo Logs       | `journalctl _COMM=sudo` | Counts executed sudo commands |

---

### Submission
Only the following files are present in the Git repository:
- `README.md`
- `signature.txt`

---

## AI Usage
AI tools were used to assist with documentation structure and to clarify project requirements, including the expected output of the `monitoring.sh` script.

## Resources
- Official Debian documentation
- Manual pages:
  - `sudo`
  - `crontab`

---

## 🌐 Network & SSH

- SSH runs on **port 4242**
- Root login via SSH is **disabled**

```bash
ssh sywee@localhost -p 4242
