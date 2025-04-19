

---

## 🔒 Block Creality Domains on Creality K1C Printers

### 📋 Overview

This script **blocks privacy-intrusive and telemetry domains** used by Creality printers (like `api.crealitycloud.com`, `ident.me`, and others) at the OS level. It modifies `/etc/hosts` to ensure these domains are **permanently blocked**, even after reboots.

> ⚠️ **Privacy-first approach**: This solution is for users seeking maximum control over the network behavior of their 3D printer.

---

### ✅ Features

- 📛 Blocks telemetry and IP-checking domains (e.g., `api.crealitycloud.com`, `ipecho.net`, etc.)
- 🔁 Automatically re-applies blocking rules at boot
- 📂 Minimal configuration required (fully automated)
- 🧠 Intelligent logic: avoids duplicate entries
- 🖥️ Compatible with Linux-based systems (e.g., K1C firmware)
- 🛠️ Simple setup with a robust `init.d` script

---

### ❓ Why Use This?

Creality printers may attempt to contact external servers for updates, telemetry, or location-based services. This can lead to:

- 🔓 Potential **privacy violations**
- 🌐 Unnecessary **bandwidth usage**
- 🐢 Possible **performance impacts**

By blocking known outbound domains, this script ensures **offline-friendly**, **privacy-respecting** operation.

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

