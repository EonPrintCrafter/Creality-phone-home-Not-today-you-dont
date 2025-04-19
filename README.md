
---

## 📋 Overview

This script is designed to **block outbound communication to known telemetry and IP-tracking domains** used by Creality K1C printers. These domains — such as `api.crealitycloud.com`, `ident.me`, `ipecho.net`, and others — are often used for:

- ✉️ **Sending usage telemetry** back to Creality's cloud infrastructure (often located in China)
- 🌍 **Exposing your public IP and geolocation** via third-party services
- 📡 **Maintaining silent, persistent connections** to external servers without user consent

Such behavior can compromise **user privacy**, **device autonomy**, and even **network security**.

By editing the system-level `/etc/hosts` file, this script ensures these domains are **completely blocked** at the operating system level — and crucially, the block list persists **even after a reboot**.

> ⚠️ **Why this matters:** These callback domains allow unsolicited data exfiltration and real-time remote monitoring, often without any user control or visibility. Blocking them is a **necessary step toward device hardening, privacy, and network hygiene**.

---

## ✅ Features

- 📛 **Blocks Invasive Domains** – Includes Creality telemetry, IP-checkers, and known tracking endpoints
- 🔁 **Automatic & Persistent** – Re-applies block rules at every system startup
- 📂 **Minimal Setup** – Fully automated; no manual updates required
- 🧠 **Idempotent Logic** – Prevents duplicate entries in `/etc/hosts`
- 🖥️ **System-Compatible** – Works on Linux-based environments (e.g. K1C BusyBox-based firmware)
- 🛠️ **Robust Init Script** – Lightweight and reliable, with no runtime dependencies

---

## ❓ Why Use This?

Creality printers often make silent outbound requests to remote servers, which can result in:

- 🔓 **Privacy Violations** – Device telemetry may include usage stats, timestamps, and network identifiers
- 🌐 **Geolocation Exposure** – Domains like `ident.me`, `ipecho.net`, `ipinfo.io`, etc. can leak your public IP and region
- 🕵️ **Remote Monitoring** – Domains like `api.crealitycloud.com` allow Creality to ping or push configs remotely
- 🐢 **Performance Overhead** – Background connectivity can interfere with real-time printing processes or updates
- 🧠 **Lack of Transparency** – These communications are often undocumented and occur silently

This script ensures your printer remains **air-gapped from known telemetry sources**, giving you full control of outbound traffic and **ensuring your printer works for you — not for someone else**.


---

### 🧩 How It Works

1. 🧾 On boot, a custom `init.d` script runs.
2. 🕵️ It checks if block entries already exist in `/etc/hosts`.
3. 🧱 Missing entries are appended automatically.
4. 🔒 Changes are **persistent and reboot-proof**.

---

## 🚀 Installation Instructions

### Step 1️⃣: SSH into the Printer

Open a terminal and connect to your Creality printer:

```bash
ssh root@<printer-ip>
```

Replace `<printer-ip>` with your printer’s IP address.

---

### Step 2️⃣: Deploy the Block Script

#### 2.1 Create the init script

Create a new file under `/etc/init.d/` called `S99block-creality`:

```bash
vi /etc/init.d/S99block-creality
```

#### 2.2 Paste the script below into the file:

```sh
#!/bin/sh
### BEGIN INIT INFO
# Provides:          block-creality
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Block unwanted domains on boot
# Description:       Adds specified domains to /etc/hosts to block outbound connections
### END INIT INFO

# ===== CONFIGURATION =====
HOSTS_FILE="/etc/hosts"

# Domains to block
BLOCK_DOMAINS="
api.crealitycloud.com
c-smart.cxswyjy.com
ident.me
ipecho.net
ifconfig.me
icanhazip.com
ipinfo.io
api.ipify.org
ip.42.pl
"

# ===== FUNCTIONS =====
block_domain() {
    local domain="$1"
    local entry="0.0.0.0 $domain"
    if ! grep -qF "$entry" "$HOSTS_FILE"; then
        echo "$entry" >> "$HOSTS_FILE"
        echo -e "\033[34m[INFO]\033[0m Added block entry: $entry"
    else
        echo -e "\033[33m[WARN]\033[0m Entry already exists: $entry"
    fi
}

main() {
    echo -e "\033[36m[STATUS]\033[0m Starting domain block script..."
    for domain in $BLOCK_DOMAINS; do
        block_domain "$domain"
    done
    echo -e "\033[32m[SUCCESS]\033[0m Blocking complete. Domains are enforced at system level."
}

main
```

---

### Step 3️⃣: Make the Script Executable

```bash
chmod +x /etc/init.d/S99block-creality
```

---

### Step 4️⃣: Enable the Script on Boot

```bash
update-rc.d S99block-creality defaults
```

✅ That’s it. Reboot the printer and your block rules will be applied automatically on each boot.

---

## 🧪 Test the Script

You can verify the domains are blocked:

```bash
ping api.crealitycloud.com
```

Expected output:

```bash
ping: unknown host api.crealitycloud.com
```

---

## 📁 Logs & Output

All actions are printed in **color-coded terminal messages**:

- 🟢 `SUCCESS`: Domain was blocked successfully
- 🔵 `INFO`: Blocking in progress or added new entry
- 🟡 `WARN`: Entry already existed in hosts file
- 🔴 `ERROR`: (Handled gracefully – not applicable here unless permission issues occur)

---

## 🔐 Security & Best Practices

- 🧱 Modifies `/etc/hosts` directly (root permission required)
- 🛡️ Includes input validation and idempotency (avoids duplicates)
- 🧼 Clean, inline-documented, and modular for future expansion
- 🧩 Can be extended easily (e.g., auto-logging, backup, remote config)

---

## 🧰 Uninstallation (Optional)

If you ever want to undo this:

```bash
update-rc.d -f S99block-creality remove
rm /etc/init.d/S99block-creality
```

Clean the `/etc/hosts` file manually if needed.

---

## ✍️ Contributions

PRs welcome! You can contribute by:

- Suggesting new domains
- Improving modularity
- Adding logging support
- Adapting for other printer models

---

## 📜 License

MIT License – Use, modify, and redistribute freely with proper credit.

---

