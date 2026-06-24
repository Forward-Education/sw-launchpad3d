# Launchpad 3D — Forward Education Fork of Kiri:Moto

This repository is [Forward Education](https://forwardedu.com)'s fork of
[Grid.Space's `grid-apps` / Kiri:Moto](https://github.com/GridSpace/grid-apps),
a browser-based slicer. It adds classroom-oriented device profiles and, most
recently, **multi-toolhead / multi-color support for Flashforge printers**.

See [CHANGELOG.md](CHANGELOG.md) for the full list of fork changes. Upstream
documentation continues below.

## Flashforge multi-tool devices

| Device | Type | Build volume | Tools |
|---|---|---|---|
| Flashforge Creator 5 Pro | 4-head **toolchanger** (FlashSwap) | 256 × 256 × 256 mm | `T0`–`T3`, physical head swap |
| Flashforge Adventurer 5X | Single nozzle + **4-color filament station** | 220 × 220 × 220 mm | `T0`–`T3`, filament unload/load + purge |
| Flashforge Adventurer 5M Pro | Single extruder | 220 × 220 × 220 mm | `T0` |

Profiles live in [`src/kiri-dev/fdm/`](src/kiri-dev/fdm/). Reference G-code
sliced from each multi-tool profile is committed at the repo root
(`flashforge-creator-5-pro-sample.gcode`, `flashforge-ad5x-sample.gcode`).

### Assigning colors / toolheads

Color/tool assignment is **per object** (Kiri:Moto does not paint regions
within a single mesh):

1. Pick the device (e.g. *Flashforge Creator 5 Pro*) in the device selector.
2. Import one mesh per color.
3. Select an object — a row of numbered buttons (**0–3**, one per toolhead)
   appears because the device declares multiple extruders. Click the number
   for the toolhead loaded with that color.
4. *(Optional)* point support material at a dedicated toolhead via the support
   **Nozzle** setting.
5. Slice and export — tool changes (`T0`–`T3`) are emitted between objects.

### ⚠️ Validation before printing

These profiles are **best-effort**. Kiri:Moto has no wipe-tower engine like
OrcaSlicer's, so multi-color purge quality (especially on the AD5X) needs
tuning on the machine, and the Creator 5 Pro toolchanger swap is unverified
against real hardware. **Diff each profile against a real slicer export for
that exact machine** before sending a job to the printer. See the *Known
limitations* section of [CHANGELOG.md](CHANGELOG.md).

### Running this fork locally

```
npm i
npm install -g @gridspace/app-server
gs-app-server --debug
```

Then open [localhost:8080/kiri](http://localhost:8080/kiri). (Full upstream
run/build instructions are below.)

---

## Grid.Space Web Applications

[![Website](https://img.shields.io/website?url=https%3A%2F%2Fgrid.space%2F)](https://grid.space/kiri/)
![GitHub package.json version (branch)](https://img.shields.io/github/package-json/v/GridSpace/grid-apps/rel-2.6)
![GitHub package.json version (branch)](https://img.shields.io/github/package-json/v/GridSpace/grid-apps/rel-2.7)
![GitHub package.json version (branch)](https://img.shields.io/github/package-json/v/GridSpace/grid-apps/rel-2.8)
![GitHub package.json version (branch)](https://img.shields.io/github/package-json/v/GridSpace/grid-apps/rel-2.9)
![GitHub package.json version (branch)](https://img.shields.io/github/package-json/v/GridSpace/grid-apps/rel-3.0)

[`Grid.Space`](https://grid.space) hosts [several live versions](https://grid.space/choose) of this code

[`Kiri:Moto`](https://grid.space/kiri) is a browser-based Slicer for 3D printers, CNC mills, and Laser cutters

[`Mesh:Tool`](https://grid.space/mesh) is a browser-based mesh repair and editing tool

## Primary Documentation

https://docs.grid.space/projects/kiri-moto

https://docs.grid.space/projects/mesh-tool

## Development Activity

![GitHub commit activity](https://img.shields.io/github/commit-activity/w/GridSpace/grid-apps)
![GitHub last commit](https://img.shields.io/github/last-commit/GridSpace/grid-apps)
![GitHub contributors](https://img.shields.io/github/contributors/GridSpace/grid-apps)

## Community Engagement

[Discord](https://discord.com/invite/suyCCgr)
 | [YouTube](https://www.youtube.com/c/gridspace)
 | [Twitter](https://twitter.com/grid_space_3d)

[![Discord](https://img.shields.io/discord/688863523207774209)](https://discord.com/channels/688863523207774209/688863523211968535)
![GitHub](https://img.shields.io/github/license/GridSpace/grid-apps)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/gridspace3d?locale.x=en_US)
![Twitter Follow](https://img.shields.io/twitter/follow/grid_space_3d?label=follow&style=social)


## Testing Locally (with Docker)

```
git clone git@github.com:GridSpace/grid-apps.git
cd grid-apps
docker-compose -f src/dock/compose.yml up
```

## Testing Locally (with NodeJS)

```
git clone git@github.com:GridSpace/grid-apps.git
cd grid-apps
npm i
npm install -g @gridspace/app-server
gs-app-server --debug
```

to start a local instance of the apps. then use a browser to open
[localhost:8080/kiri](http://localhost:8080/kiri)

if installing the app-server fails or gives you permissions errors, then your node installation (on linux/mac) is installed as another user (like root). try instead:

```
sudo npm install -g @gridspace/app-server
```

Alternatively, if you are using a packaged version of npm that ships with
a Linux distribution, but still want to install in your home directory, you
can use

```
npm config set prefix ~/.local
```

If gs-app-server is not found, then perhaps ~/.local/bin is not in
your path. You can either add it to your path, or you can run:

```
~/.local/bin/gs-app-server --debug
```

You can now access your environment of grid-apps by going to
[localhost:8080/kiri](http://127.0.0.1:8080/kiri)

## Windows Developers

this git repo requires symbolic link support. on Windows, this means you have to clone the repo in a command shell with Administrator privileges.

## Other Start Options

```
gs-app-server
```
serves code as obfuscated, compressed bundles. this is the mode used to run on a public
web site.

requires node.js 12+

## Javascript Slicing APIs

A script include that injects a web worker into the page that will asynchronously perform any of Kiri’s slicing and gcode generation functions. And a frame messaging API for controlling Kiri:Moto inside an IFrame.

* https://grid.space/kiri/engine.html
* https://grid.space/kiri/frame.html
