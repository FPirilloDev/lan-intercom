# Topology & setup — LAN Intercom

## Network topology

```
┌────────────────────┐
│  ISP modem/router  │  (provides internet + DHCP)
└─────────┬──────────┘
          │
┌─────────▼──────────┐
│  Tenda SG105       │  5-port unmanaged Gigabit switch (10/100/1000),
│  Gigabit switch    │  auto MDI/MDIX (no crossover cable needed)
└───┬────────────┬───┘
    │            │
┌───▼───┐   ┌────▼────┐
│Desktop│   │ Laptop  │   Both on the same subnet → mutual visibility + internet
└───────┘   └─────────┘
```

## Software stack

| Layer | Choice |
|---|---|
| Shared mouse/keyboard/clipboard | Microsoft PowerToys → **Mouse Without Borders** (same version on both) |
| File sharing (folder) | Native Windows **SMB** (File and Printer Sharing) |
| File transfer (single file) | Drag-and-drop via Mouse Without Borders |
| OS | Windows 11 on both machines |

## How pairing works

Mouse Without Borders pairs the two machines with a **security key** generated on
the primary and entered on the secondary. Communication is **LAN-only** (no
internet). Hosts resolve each other by **computer name**, so the link survives the
router assigning a different IP.

- Shared folder path pattern: `\\[HOST]\[shared-folder]` (host name redacted).

## Known issue & diagnosis

**Symptom:** the laptop froze — taskbar and app-switching stopped responding.
**Root cause (diagnosed by the author):** it happened when opening a terminal or any
window **as administrator**. Windows **UAC privilege isolation** prevents a
normal-privilege process from injecting input into an elevated window, which froze
the shared peripherals.
**Direction of fix:** keep privilege levels consistent for the shared-control tool /
avoid elevated windows while shared control is active.

<!-- ADD:notes -->
