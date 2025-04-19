# Block Creality Domains on K1C Printers

## Overview
This script blocks certain unwanted domains (such as `api.crealitycloud.com`, `c-smart.cxswyjy.com`, and others) by modifying the `/etc/hosts` file on your **Creality K1C** printer. It ensures that these domains are blocked after every reboot, preventing unwanted connections to external servers.

This solution is ideal for users who want to maintain control over their K1C printer's internet traffic and block access to external domains that may compromise privacy or performance.

## Features
- **Prevents unwanted domains** like `api.crealitycloud.com`, `c-smart.cxswyjy.com`, `ident.me`, `ipecho.net`, and more from being accessed.
- **Auto-checks and updates** the `/etc/hosts` file every time the device reboots.
- **Works for Creality K1C** and similar devices running Linux-based systems.
- **Simple setup** with a single init script.

## Why Do You Need This?
Creality K1C printers come with pre-configured access to various online domains (like Creality’s servers) that can affect your privacy or printer performance. This script ensures that these domains are blocked at the system level, preventing unnecessary or unwanted communication with external servers.

## How It Works
- The script checks if the domains to block already exist in the `/etc/hosts` file.
- If they don’t exist, it adds them to the file to block them.
- The script runs automatically on system startup to ensure the entries persist after reboots.

## Installation Instructions
### Step 1: Access your K1C Printer
1. SSH into your K1C printer using the terminal (or command line).
   ```bash
   ssh root@<printer-ip>
   

---

#### **Step 2.2: Create the Script File (`S99block-creality`)**

1. **Create a new file** in GitHub called `S99block-creality`.

2. **Paste the following script** into `S99block-creality`:

```sh
#!/bin/sh

# Define the block entries and the file
HOSTS_FILE="/etc/hosts"

# List of domains to block
BLOCK_ENTRY_1="0.0.0.0 api.crealitycloud.com"
BLOCK_ENTRY_2="0.0.0.0 c-smart.cxswyjy.com"
BLOCK_ENTRY_3="0.0.0.0 ident.me"
BLOCK_ENTRY_4="0.0.0.0 ipecho.net"
BLOCK_ENTRY_5="0.0.0.0 ifconfig.me"
BLOCK_ENTRY_6="0.0.0.0 icanhazip.com"
BLOCK_ENTRY_7="0.0.0.0 ipinfo.io"
BLOCK_ENTRY_8="0.0.0.0 api.ipify.org"
BLOCK_ENTRY_9="0.0.0.0 ip.42.pl"

# Function to check and add the entry
block_entry() {
    local entry="$1"
    if ! grep -qF "$entry" "$HOSTS_FILE"; then
        echo "$entry" >> "$HOSTS_FILE"
        echo "[INFO] Added block entry: $entry"
    else
        echo "[INFO] Block entry already exists: $entry"
    fi
}

# Check and add all block entries
block_entry "$BLOCK_ENTRY_1"
block_entry "$BLOCK_ENTRY_2"
block_entry "$BLOCK_ENTRY_3"
block_entry "$BLOCK_ENTRY_4"
block_entry "$BLOCK_ENTRY_5"
block_entry "$BLOCK_ENTRY_6"
block_entry "$BLOCK_ENTRY_7"
block_entry "$BLOCK_ENTRY_8"
block_entry "$BLOCK_ENTRY_9"
