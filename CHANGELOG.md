# Changelog — Forward Education Fork

Changes specific to this fork of [Kiri:Moto / grid-apps](https://github.com/GridSpace/grid-apps).
Upstream release notes live in [release.md](release.md).

The format is based on [Keep a Changelog](https://keepachangelog.com/).
Dates are ISO‑8601 (YYYY‑MM‑DD).

## [Unreleased]

### Added — Flashforge multi-tool support (2026-06-24)

- **New device: Flashforge Creator 5 Pro** (`src/kiri-dev/fdm/Flashforge.Creator.5.Pro`).
  4-head **toolchanger** (FlashSwap). Build volume 256 × 256 × 256 mm, 0.4 mm
  nozzle, 1.75 mm filament. Four extruders selecting `T0`–`T3` with zero XY
  offset (the toolchanger compensates head offsets in firmware). Start/end
  gcode preheats and cools **only the toolheads used in a given print** via
  `;; IF { tool_used_N }` conditionals. Purge tower disabled (the machine
  manages material switching).
- **New device: Flashforge Adventurer 5X** (`src/kiri-dev/fdm/Flashforge.Adventurer.5X`).
  Single 0.4 mm nozzle fed by a **4-color filament station (CFS)** — not a
  toolchanger. Build volume 220 × 220 × 220 mm. Color changes emit an
  unload (`G1 E-5`) → `Tn` → load (`G1 E5`) sequence plus a purge block.
  Authored from a real **OrcaSlicer 2.3.2** reference export for the AD5X, so
  the start, toolchange, and end blocks match the machine's expected dialect.
- **Sample G-code** generated from each profile and committed for reference:
  `flashforge-creator-5-pro-sample.gcode`, `flashforge-ad5x-sample.gcode`
  (each slices two boxes assigned to different toolheads to exercise a tool
  change).
- **`.claude/launch.json`** dev-server launch configuration (`gs-app-server`
  on port 8080).

### Changed

- **Confirmed Flashforge / Klipper dialect** applied consistently across the
  Flashforge profiles, derived from the real AD5X export:
  `HEADER_BLOCK`/`EXECUTABLE_BLOCK` markers, `M73` progress reporting, primary
  + auxiliary fans (`M106 S…` / `M106 P2 S…`), `SET_PRESSURE_ADVANCE`,
  per-layer `SET_VELOCITY_LIMIT` (500 on the first layer, 10000 after), and
  `Tn` tool/filament selection.
- **Creator 5 Pro origin** switched from centered to **corner origin**
  (`originCenter: false`, coordinates 0..bed) and its prime line moved to the
  back edge, to match the coordinate convention confirmed by the AD5X
  reference.
- **Device images**: added product photos `web/kiri/img/creator5pro.png` and
  `web/kiri/img/ad5x.png` and wired them into the two profiles (previously
  both reused the AD5M Pro image).
- Renamed the Creator 5 profile to **Creator 5 Pro**
  (`Flashforge.Creator.5` → `Flashforge.Creator.5.Pro`, display name and
  sample G-code renamed to match).

### Known limitations / validation

- Kiri:Moto has **no wipe-tower engine** equivalent to OrcaSlicer's. For the
  AD5X (single nozzle, multiple filaments) color quality depends on the purge
  block + load/unload macros; tune `outputPurgeTower` and the load/unload
  amounts on the machine.
- The **Creator 5 Pro toolchanger swap is best-effort** — no reference G-code for
  that machine was available. It assumes the firmware binds toolchange macros
  to `T0`–`T3`.
- Before printing, **diff each profile against a real slicer export** for that
  exact machine and reconcile the start / toolchange / end blocks.

---

## Earlier fork additions

Device profiles and fixes added to this fork prior to the multi-tool work
(see `git log` for detail):

- Flashforge Adventurer 5M Pro device profile and product image.
- InkSmith MakerForge and InkSmith Orbit device profiles (incl. a dwell/`G4`
  crash fix for the Orbit).
- Cubicon Style, Style Neo, Style Plus, and Single Plus device profiles.
- UI tweaks: `/` route redirects to `/kiri`, device images precache, device
  selection made more noticeable, custom-printer button removed.
