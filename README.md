# ☎️ Creality “Phone Home” – Not Today, You Don’t™

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platform: K1 Series Printers](https://img.shields.io/badge/platform-K1_Series--BusyBox-lightgrey.svg)](https://github.com/EonPrintCrafter/Creality-phone-home-Not-today-you-dont)
[![Downloads: 999+ soon 🚀](https://img.shields.io/badge/downloads-999%2B_soon-%F0%9F%9A%80-brightgreen.svg)](https://github.com/EonPrintCrafter/Creality-phone-home-Not-today-you-dont/releases)
---

## 🧭 Overview

Your Creality K1C printer is stealthily chatting with the internet — sending telemetry, leaking IPs, and phoning home without permission. This script slams the door shut by rerouting those domains to `0.0.0.0` via `/etc/hosts`, and keeps it shut—even after reboot.

No smoke, no mirrors: it’s a **one-minute install** to regain control of your printer’s privacy 😎.

---

## 🚀 Features & Perks

- 🛡️ **Blocks telemetry & tracking endpoints**  
- 🌀 **Auto-persistence at boot** with `init.d`  
- 🔍 **Smart logic** — no duplicate host entries  
- ⚙️ **Zero maintenance** — install once, forget it  
- 🧰 **No external dependencies** — pure POSIX shell  
- 💾 **Tiny footprint** — ideal for busybox/embedded systems  

---

## 🤔 Why Should You Care?

Because every ping and telemetry payload:

- 🔓 Leaks print data, time, location…  
- 🌐 Exposes your printer’s IP and geolocation  
- 📡 Enables covert remote monitoring  
- 🐢 May slow down other processes  
- 🤷‍♂️ Happens silently, with no consent  

Take back control — your printer prints for you, not for “them.”

---

## ⚔️ One-Liner Install (As Root)

```sh
sh -c 'cat <<EOF >/etc/init.d/S99block-creality
#!/bin/sh
HOSTS_FILE="/etc/hosts"
block_entry(){
  e="\$1"
  grep -qF "\$e" "\$HOSTS_FILE" || { echo "\$e" >> "\$HOSTS_FILE"; echo "[INFO] Added block entry: \$e"; return; }
  echo "[INFO] Already blocked: \$e"
}
for d in \
"0.0.0.0 api.crealitycloud.com" \
"0.0.0.0 c-smart.cxswyjy.com" \
"0.0.0.0 ident.me" \
"0.0.0.0 ipecho.net" \
"0.0.0.0 ifconfig.me" \
"0.0.0.0 icanhazip.com" \
"0.0.0.0 ipinfo.io" \
"0.0.0.0 api.ipify.org" \
"0.0.0.0 ip.42.pl"; do block_entry "$d"; done
EOF
chmod +x /etc/init.d/S99block-creality && update-rc.d S99block-creality defaults'
```

---

## ✅ Test It After Reboot

```sh
ping api.crealitycloud.com
# Expected: ping: unknown host api.crealitycloud.com
```

That’s a **success** — no surprise phone call ever made 🙂

---

## 🧰 Manual Install (If You Want More Control)

1. SSH in: `ssh root@<printer-ip>`
2. `vi /etc/init.d/S99block-creality`
3. Paste the same block logic as above
4. `chmod +x /etc/init.d/S99block-creality`
5. `update-rc.d S99block-creality defaults`

All done! 🎉

---

## 🧹 Uninstall (If You Dare to Go Back)

```sh
update-rc.d -f S99block-creality remove
rm /etc/init.d/S99block-creality
# Then manually purge lines from /etc/hosts
```

---

## 📝 Contributions Welcome

Want in?

- 🚀 Add new domains to block  
- 🧩 Port to other printers/firmware  
- 🐞 Enhance logging, add features  

PRs, issues, or constructive rants welcome!

---

## 📜 License

MIT — **Copy, remix, obliterate** (just keep the license). Attribution appreciated.

---

## 💬 Talk to Us

Shoot your questions, suggestions—or jokes—here:  
👉 [GitHub Discussions & Issues](https://github.com/EonPrintCrafter/Creality-phone-home-Not-today-you-dont/issues)

---

> *“I prefer my printer silent. The only sound I want is the sizzling hot nozzle, not telemetry bursts.”*

---

Rock on, printer warriors. May your prints be flawless—and your network silent. 🔇

