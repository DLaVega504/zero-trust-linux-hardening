# Universal Zero-Trust Linux Hardening Framework

An enterprise-grade, multi-layer host security and system optimization baseline implemented via Ansible. This configuration applies identical hardening policies across diverse Linux distributions, optimizing memory allocations while systematically reducing target system attack surfaces.

## 📊 Lynis Security Audit Benchmarks
The design baseline achieves high-security compliance scores across multiple distinct system environments without breaking operational user-space workflows:

| Operating System | Base Filesystem | Hardening Index Score |
| :--- | :--- | :--- |
| **openSUSE Leap** | Btrfs + Snapper | **92** |
| **Debian GNU/Linux** | Btrfs / Ext4 | **90** |
| **Arch Linux** | Custom FDE + BlackArch | **81** |

---

## 🛡️ Core Defensive Architecture Layers

### 1. Hybrid Memory Layout & Stability
* **Volatile Tier Optimization:** Provisions a high-priority systemd-managed ZRAM swap space (`priority=100`) using optimized `lz4` compression engines to manage execution pools entirely in volatile memory.
* **Backing Store Safety:** Sniffs out the underlying root filesystem type. Automatically builds an atomic, unfragmented, non-CoW backing swapfile using native `btrfs filesystem mkswapfile` variables, or falls back to traditional contiguous allocations on generic filesystems.
* **OOM Resource Protection:** Deploys strict user-space `earlyoom` threshold controls to kill aggressive memory leaks before they trigger fatal kernel panics.

### 2. Network Integrity Sharding
* **Arpwatch Daemons:** Deploys template-driven systemd overrides (`arpwatch@.service.d`) to bind execution streams securely under strict service user restrictions across all physical interfaces.
* **Firewalld Baseline:** Lock-down network topologies using hardened firewall filtering rule matrices.

### 3. File System Auditing & Intrusion Detection
* **Cryptographic Baselines:** Deploys automated `AIDE` file system monitoring engines to hash and alert on tampering variants across system administrative endpoints.
* **Auditd Subsystem Tracking:** Implements explicit kernel-level process tracking rules targeting user privileges (`/etc/passwd`, `/etc/group`) and dynamic runtime module insertions (`insmod`, `rmmod`).
* **Resource Clamping:** Throttles heavy operational security scanners (like `ClamAV`) inside strict systemd cgroup slices to guarantee memory limits remain below `1.2G` with a strict `10%` maximum CPU quota.

---

## 🗂️ Repository Directory Blueprint
```text
hardened-linux-playbook/
├── ansible.cfg          # Optimized privilege escalation and YAML output rules
├── hosts.ini            # Secure local localhost execution token definition
├── vars_mapping.yml     # Decoupled platform package mappings and system paths
└── Linux-Hardening.yml  # Main universal infrastructure orchestration playbook
```

## 🚀 Local Deployment Execution
To parse the syntax and apply the security architecture baseline execution to your local host node:

```bash
# 1. Execute a pre-flight syntax check
ansible-playbook -i hosts.ini Linux-Hardening.yml --syntax-check

# 2. Run the secure production deployment pipeline
ansible-playbook -i hosts.ini Linux-Hardening.yml
```

