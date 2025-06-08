# 🛡️ Creality "Phone Home" Blocker – Not Today, You Don’t

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

## 🔐 Security & Best Practices

- ✅ Modifies `/etc/hosts` directly (root required)  
- ✅ Clean, POSIX-compliant logic (no external deps)  
- ✅ Prevents duplicates (idempotent)  
- ✅ Easy to audit, extend, or rollback  

---

## 🚀 Instant Deployment (One-Liner)

Run this as root via SSH on your printer:

```sh
sh -c 'cat <<EOF > /etc/init.d/S99block-creality
#!/bin/sh
HOSTS_FILE="/etc/hosts"
block_entry() {
  entry="$1"
  grep -qF "$entry" "$HOSTS_FILE" || { echo "$entry" >> "$HOSTS_FILE"; echo "[INFO] Added block entry: $entry"; return; }
  echo "[INFO] Block entry already exists: $entry"
}
block_entry "0.0.0.0 api.crealitycloud.com"
block_entry "0.0.0.0 c-smart.cxswyjy.com"
block_entry "0.0.0.0 ident.me"
block_entry "0.0.0.0 ipecho.net"
block_entry "0.0.0.0 ifconfig.me"
block_entry "0.0.0.0 icanhazip.com"
block_entry "0.0.0.0 ipinfo.io"
block_entry "0.0.0.0 api.ipify.org"
block_entry "0.0.0.0 ip.42.pl"
EOF
chmod +x /etc/init.d/S99block-creality && update-rc.d S99block-creality defaults'
```

---

## 🧪 Test the Block

After rebooting the printer:

```sh
ping api.crealitycloud.com
```

Expected output:

```sh
ping: unknown host api.crealitycloud.com
```

---

## 🧰 Manual Install (Alternative to One-Liner)

### Step 1️⃣: SSH into the Printer

```bash
ssh root@<printer-ip>
```

### Step 2️⃣: Create Init Script

```bash
vi /etc/init.d/S99block-creality
```

Paste the script:

```sh
#!/bin/sh
HOSTS_FILE="/etc/hosts"
block_entry() {
  entry="$1"
  grep -qF "$entry" "$HOSTS_FILE" || { echo "$entry" >> "$HOSTS_FILE"; echo "[INFO] Added block entry: $entry"; return; }
  echo "[INFO] Block entry already exists: $entry"
}
block_entry "0.0.0.0 api.crealitycloud.com"
block_entry "0.0.0.0 c-smart.cxswyjy.com"
block_entry "0.0.0.0 ident.me"
block_entry "0.0.0.0 ipecho.net"
block_entry "0.0.0.0 ifconfig.me"
block_entry "0.0.0.0 icanhazip.com"
block_entry "0.0.0.0 ipinfo.io"
block_entry "0.0.0.0 api.ipify.org"
block_entry "0.0.0.0 ip.42.pl"
```

### Step 3️⃣: Make It Executable

```bash
chmod +x /etc/init.d/S99block-creality
```

### Step 4️⃣: Enable on Boot

```bash
update-rc.d S99block-creality defaults




---

## 🧼 Uninstall

```bash
update-rc.d -f S99block-creality remove
rm /etc/init.d/S99block-creality
# Optionally: manually clean /etc/hosts
```

---

## ✍️ Contributions Welcome

Feel free to:

- 📡 Suggest new telemetry domains  
- 📜 Improve modularity or add logging  
- 🧪 Extend to other firmware types or devices  

---

## 📜 License

**MIT License** — Use, modify, and redistribute freely with proper attribution.

---

## 💬 Questions?

Open an issue or start a discussion:  
👉 https://github.com/EonPrintCrafter/Creality-phone-home-Not-today-you-dont/issues
