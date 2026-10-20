# Minecraft Server: All the Mods 11

A self-hosted, modded Minecraft server for a small private group. It's the first live service in my homelab and a hands-on way to practice service administration, remote access, firewall configuration, and troubleshooting.

## Overview

| | |
|---|---|
| **Modpack** | All the Mods 11 (ATM11), beta |
| **Minecraft / Loader** | Minecraft 26.1.2 · NeoForge |
| **Runtime** | Eclipse Temurin JDK 25 |
| **Host** | HP Pavilion Gaming 15 (i5-9300H, 16GB DDR4), Windows 11 |
| **Server path** | `C:\mcserver` |
| **Remote access** | Tailscale (no ports exposed to the internet) |
| **Players** | 2 (whitelisted) |

## Architecture

```
 Friend's PC ──┐
               │   Tailscale (encrypted WireGuard mesh, 100.x.x.x)
 My main PC ───┼──────────────────────────────►  HP Pavilion laptop
               │                                  └─ ATM11 server :25565
```

- No port forwarding on the home router. The server is only reachable by devices on my Tailscale network or explicitly shared with it.
- The laptop is shared with my friend's Tailscale account through the admin console, not through a public address.

## Setup

1. **Installed Java:** Temurin JDK 25 from adoptium.net, with `JAVA_HOME` and `PATH` set. Verified with `java -version`.
2. **Server files:** downloaded the official ATM11 server pack from CurseForge and unzipped it to `C:\mcserver` (kept out of OneDrive-synced folders).
3. **First run:** ran the start script to install NeoForge, then accepted the EULA (`eula=true`).
4. **Memory:** set heap size in `user_jvm_args.txt` (currently `-Xmx8G`, sized to leave headroom for Windows and future VMs).
5. **Access control:**
   - `white-list=true` in `server.properties`, with players added via `whitelist add <username>`
   - `online-mode=true`, so only legitimate Minecraft accounts can join
   - Operator status granted only to me (`op <username>`)
6. **Remote access:** installed Tailscale, signed in, and shared the machine with my friend from the Tailscale admin console.
7. **Host configuration:**
   - Allowed Java through Windows Defender Firewall
   - Never sleep when plugged in, and lid close set to "Do nothing"
   - Windows Update active hours set to avoid restarts during play

## Migration: Main PC → Lab Host (Oct 2026)

The server originally ran on my main desktop (Ryzen 5 5600X, 16GB). Running the ATM11 client and server on the same machine pushed memory usage above 90%, so I moved the server to a dedicated host.

1. Stopped the server cleanly with `stop` so the world fully saved.
2. Zipped the **entire** server folder (world, mods, config, `server.properties`, `whitelist.json`, `ops.json`, JVM args) and moved it by USB.
3. Installed Java 25 on the laptop and unzipped to `C:\mcserver`.
4. Upgraded the laptop from 8GB to 24GB RAM and adjusted the heap size to match.
5. Installed Tailscale on the laptop and re-shared it with my friend, since the old share pointed to the desktop.
6. Tested by joining from my desktop over Tailscale before retiring the old copy.

**Result:** the desktop no longer runs the server, and the server can stay up independently of my gaming PC.

## Troubleshooting Log

| Problem | Cause | Fix |
|---|---|---|
| "Incompatible version" on join | Launched vanilla Minecraft instead of the ATM11 CurseForge profile | Launch from the modded profile, matching the server's pack version |
| Friend got "Connection timed out" | Friend accepted the Tailscale share in a browser but never installed the Tailscale client | Installed the client and signed in with the account that accepted the share |
| Main PC memory hit 92% | ATM11 client and server running on the same 16GB machine | Migrated the server to a dedicated host |
| Could connect on Wi-Fi but not Ethernet | Windows assigned the Ethernet connection a different network profile, and the firewall rule didn't cover it | _Fix: TBD_ |
| Friend's client JVM crashing on join | Client-side issue (memory allocation / drivers) | Lowered client RAM from 8GB to 6GB; further steps: GPU driver update, clean JDK install |

**Diagnostic approach I use:** ping the host's Tailscale IP first. No reply means a Tailscale/network problem. Replies mean the issue is the firewall or the Minecraft server itself.

## Security Considerations

- **No exposed ports:** remote access goes through Tailscale instead of port forwarding, so the server isn't discoverable by internet-wide scanners.
- **Least privilege:** whitelist plus `online-mode=true`, and op limited to one admin account.
- **Firewall scope:** Java is allowed through the Windows firewall. A tighter option I'm evaluating: set only the Tailscale interface to Private and restrict the rule to Private networks.
- **No secrets in this repo:** world data, mod jars, logs, and backups are excluded via `.gitignore`. Mods are not redistributed; the pack is available from CurseForge.

## Roadmap

- [ ] Automated, timestamped world backups (PowerShell + Task Scheduler) with a tested restore
- [ ] Run the server as a background service that starts on boot and restarts on crash
- [ ] Pre-generate chunks to reduce lag spikes
- [ ] Restrict the firewall rule to the Tailscale interface only
- [ ] Monitoring: uptime checks and resource usage
- [ ] Move to a Linux VM under Proxmox

## Lessons Learned

- Remote access issues are usually one of three layers: the VPN link, the host firewall, or the application. Testing each layer in order (ping → firewall → server) finds the problem fastest.
- Windows network profiles (Public vs. Private) silently change which firewall rules apply. A new adapter can break a working setup.
- Separating the server from the gaming client freed up resources on both, and was the main reason for building a dedicated lab host.
