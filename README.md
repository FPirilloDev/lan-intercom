# 🖥️🔌🖥️ LAN Intercom — Two-PC Shared Workstation

**A home networking setup that makes two independent Windows PCs behave partly as a
single workstation:** one shared mouse & keyboard across both screens, a shared
clipboard, drag-and-drop file transfer, and a shared network folder over the LAN.

> 🇦🇷 Versión en español → [README.es.md](README.es.md)
>
> ℹ️ **This is a documentation project, not software** — no code, database or
> deployment. It documents a real setup that was configured and verified hands-on.

<!-- ADD:badges -->
`Windows 11` · `PowerToys — Mouse Without Borders` · `SMB file sharing` ·
`Gigabit switch (Tenda SG105)` · `LAN`

---

## What it does

- **Shared mouse & keyboard.** The cursor crosses from one screen to the other at
  the edge, and the keyboard follows the cursor. Either machine can take control
  with its own physical peripheral.
- **Shared clipboard.** Text copied on one machine pastes on the other.
- **File transfer** by drag-and-drop (single files) and a **shared network folder**
  (SMB) mounted as a network drive.
- **Multi-monitor-style layout.** Physical arrangement configured so the cursor
  respects the screen edges instead of wrapping around.

## How it works

```
ISP modem/router ──► Tenda SG105 (Gigabit switch) ──► Desktop PC
                                              └─────► Laptop
```

- **Topology:** the switch hangs off the ISP router, so both PCs share the same
  subnet — keeping internet and DHCP while gaining mutual visibility.
- **Software:** Microsoft **PowerToys** (Mouse Without Borders module) on both
  machines, paired with a security key; **native Windows SMB** for the shared
  folder. Communication is purely local — no internet required. Machines resolve
  each other by computer name, so it reconnects even if the router hands out a new IP.

More detail: [`docs/topology.md`](docs/topology.md)

## A real problem solved (troubleshooting)

The laptop would freeze (taskbar and app-switching unresponsive). Root cause,
**diagnosed by the author**: it happened when opening a terminal/window **as
administrator** — Windows UAC privilege isolation blocks a normal-privilege process
from injecting input into an elevated window, freezing the shared peripherals. The
fix path follows from that diagnosis (run the pairing tool with matching privilege
level / avoid elevated windows during shared control).

## Status

Functional and in use. Shared mouse/keyboard, clipboard and drag-drop work. The SMB
shared folder is partially configured (a pending credentials prompt). <!-- ADD:status -->

## Key decisions

- **No expensive switch.** A managed PoE switch was considered and dropped — PoE and
  management were irrelevant here; an unmanaged Gigabit Tenda SG105 was enough.
- **Switch off the router, not isolated.** Keeps internet + DHCP (no manual IPs).
- **Mouse Without Borders over Input Leap/Barrier.** Both machines are Windows, so
  the built-in Microsoft tool was simpler and supports file drag.
- **Expectation set:** you cannot move *windows* between two independent OSes — only
  peripherals, clipboard and files are shared.

## Authorship

**Directed and executed by the author; technical assistance by AI.** The author
defined the goal, performed every change on his machines, verified each step, and
**diagnosed the freeze issue himself** (the AI had not identified it). The AI
provided the technical know-how and step-by-step instructions.

## License

[MIT](LICENSE) · _Set the copyright holder to your name/GitHub handle._
