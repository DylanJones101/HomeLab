# HomeLab

A personal cybersecurity and networking lab where I build, break, secure, and document real infrastructure. I'm a Computer Science student at Washington State University focused on network security, and this lab is where I turn coursework and certification study into hands-on experience.

## Goals

- Build and manage self-hosted services like a real sysadmin would
- Design and segment a network with VLANs and firewall rules
- Deploy intrusion detection and monitoring (Suricata / Zeek)
- Harden IoT devices and analyze their traffic
- Document everything: what I built, what broke, and how I fixed it

## Hardware

| Device | Role | Specs |
|---|---|---|
| HP Pavilion Gaming 15 (15-dk0068wm)| Main lab host (services + VMs) | CPU: Intel Core i5-9300h (4-cores/ 8 threads) · RAM: 24GB DDR4 · Storage: 256GB SSD |

_More hardware will be added as the lab grows._

## Current Services

| Service | Status | Description |
|---|---|---|
| [Minecraft Server (All the Mods 11)](services/minecraft-server/) | 🟢 Running | Self-hosted modded game server. Migrated from my main PC to the lab host. Focus on uptime, backups, resource tuning, and secure remote access. |

## Roadmap

- [x] Migrate Minecraft server to dedicated lab host
- [x] Set up this repository for documentation
- [ ] Automated server backups and restore testing
- [ ] Install a hypervisor and build lab VMs
- [ ] Design network segmentation (VLANs) and firewall rules
- [ ] Network topology diagram
- [ ] IDS/IPS monitoring box with Suricata and Zeek
- [ ] IoT sensor build with security hardening and traffic capture

## Repository Structure

```
HomeLab/
├── diagrams/          Network topology and architecture diagrams
├── docs/              Lab journal and incident notes
├── network/           VLAN plan, IP scheme, firewall rules
├── scripts/           Automation (backups, updates, health checks)
├── security/          Hardening steps and access control
├── services/          One folder per self-hosted service
└── virtualization/    Hypervisor setup and VM inventory
```

## Skills Demonstrated

- **Systems administration:** service management, resource tuning, backups
- **Networking:** port forwarding, firewall configuration, segmentation (in progress)
- **Security:** hardening, least-privilege access, intrusion detection (planned)
- **Documentation:** change logs, incident write-ups, architecture diagrams

## Certifications In Progress

- CompTIA A+
- CompTIA Network+
- CompTIA Security+

## Lab Journal

Ongoing changes and lessons learned are logged in [docs/lab-journal.md](docs/lab-journal.md).

---

> All IPs, hostnames, and credentials in this repo are sanitized or placeholders. No real secrets are stored here.
