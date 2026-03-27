
````markdown
# Optimization Scripts for Linux
# follow up readme
A collection of Linux optimization scripts focused on improving **system memory performance** using **ZRAM, tmpfs, and kernel tweaks**.  

This repository currently includes a **self-installing script** for Debian-based systems like **Parrot OS, Ubuntu, and Debian**.

It is designed for **sysadmins, power users, and performance enthusiasts** who want to maximize system responsiveness on machines with limited RAM by leveraging **compressed memory and RAM-backed filesystems**.

---

## Contents

| Script | Description |
|--------|-------------|
| `zramoptimization.sh` | Automatically configures **ZRAM swap**, **tmpfs for /tmp**, and kernel performance tunables on Debian-based systems |

---

## Features

The **ZRAM Optimization Script** provides the following:

- Detects **total system RAM** and calculates optimal allocations for:
  - **Compressed RAM swap (ZRAM)**
  - **RAM-backed tmpfs for /tmp**
- Installs `zram-tools` if not already present
- Configures `/etc/default/zramswap` automatically
- Applies kernel memory optimizations (`vm.swappiness` and `vm.vfs_cache_pressure`)
- Mounts `/tmp` as a **tmpfs** using approximately 50% of total RAM
- Creates and registers a **systemd service** to run the script automatically at boot
- Displays live feedback for:
  - Swap usage
  - ZRAM devices
  - `/tmp` tmpfs usage

---

## Installation Instructions

Follow these steps to install and enable the script on **Parrot OS / Debian-based systems**.

### 1. Clone the repository

```bash
git clone https://github.com/realkevo/optimizationscripts.git
cd optimizationscripts
````

### 2. Save the script

Create the script file inside the repository:

```bash
nano zramoptimization.sh
```

Paste the **full script contents** (the self-copying, systemd-enabled version).
Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

### 3. Make the script executable

```bash
chmod +x zramoptimization.sh
```

### 4. Run the script

```bash
sudo ./zramoptimization.sh
```

**What happens when the script runs:**

* Copies itself to `/usr/local/bin`
* Configures **ZRAM swap** and `/tmp` as tmpfs
* Installs and enables a **systemd service**
* Displays the current memory configuration status

### 5. Manage the systemd service

The script automatically creates:

```text
/etc/systemd/system/zramoptimization.service
```

You can manually control the service with:

```bash
sudo systemctl start zramoptimization.service
sudo systemctl enable zramoptimization.service
sudo systemctl status zramoptimization.service --no-pager
```

---

## Verify System Optimization

Check active memory and swap statistics:

```bash
swapon --show
zramctl
df -h /tmp
```

**Expected results:**

* `/dev/zram0` should be listed as a **swap device**
* `/tmp` should be mounted as a **tmpfs RAM-backed filesystem**

**Example output:**

```text
NAME       TYPE      SIZE   USED PRIO
/dev/zram0 partition 2.8G  4.3M   100

Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1.9G   26M  1.9G   2% /tmp
```

---

## Uninstall

To remove the script and cleanup system modifications:

```bash
sudo systemctl stop zramoptimization.service zramswap.service
sudo systemctl disable zramoptimization.service zramswap.service
sudo rm -f /usr/local/bin/zramoptimization.sh
sudo rm -f /etc/systemd/system/zramoptimization.service
sudo rm -f /etc/default/zramswap
sudo systemctl daemon-reload
```

---

## Requirements

* Debian / Ubuntu / Parrot OS or derivative
* `bash`, `systemd`, and `sudo` privileges

---

## Contributions

Contributions are welcome!
Feel free to submit **issues** or **pull requests** to improve the script or add new optimization tools.


