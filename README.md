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
   
### Step 2: Download the Script
Clone this repository to your K1C printer:
   cd /etc/init.d
   wget https://github.com/your-repo/block-creality-domains.git

### Step 3: Set Up the Script
Copy the script to /etc/init.d/:
   cp /path/to/S99block-creality /etc/init.d/S99block-creality

Make the script executable:
   chmod +x /etc/init.d/S99block-creality

### Step 4: Test the Script
Run the script manually to test:
   /etc/init.d/S99block-creality

### Step 5: Reboot Your Printer
The script will run automatically on every reboot to block the specified domains.
   reboot

## Customization
You can modify the list of domains to block by editing the S99block-creality script. Simply add or remove entries from the script as needed.
