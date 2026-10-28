![Dragonfly Hall on MPC](https://raw.githubusercontent.com/gmorb/mpc-vst-dragonfly-hall/main/docs/banners/hall.png)

# Dragonfly Hall for MPC OS

An unofficial port of the [Dragonfly Reverb](https://github.com/michaelwillis/dragonfly-reverb) **Hall Reverb** plugin (3.2.10) by Michael Willis and Rob van den Berg, as a native insert effect for Akai MPC and Akai Force standalone devices, **Gen1** (32-bit: Force, MPC Live, Live II, One, X, Key 61) and **Gen2** (64-bit: MPC Live III, One G2, Key G2). A touchscreen page modelled on the original plugin's UI, Q-Link mapping, the original's presets, and full project recall.

A lush hall reverb: early reflections plus a modulated late reverb, with a two-band decay (low and high multipliers around adjustable crossovers). 25 presets in 5 banks, from rooms and studios to small, medium and large halls.

The DSP is upstream's own code, compiled with upstream's own flags, and null-tested against upstream's binary: see [Sound](https://github.com/gmorb/mpc-vst-dragonfly/blob/v1.3.0/docs/SOUND.md).

It shows up in MPC's plugin list as **Hall**, manufacturer **Dragonfly**.

This repo is the plugin's listing in the [MPC OS Plugin Catalog](https://sd88me.github.io/mpc-vst-plugins/). **The source code is in [gmorb/mpc-vst-dragonfly](https://github.com/gmorb/mpc-vst-dragonfly)**, which builds all four Dragonfly reverbs; each release here links to the exact source tag it was built from.

## Install

From the [MPC OS Plugin Catalog](https://sd88me.github.io/mpc-vst-plugins/) (its installers pick the right build for your device), or download from [Releases](../../releases/latest):

| Device | Download |
|---|---|
| Gen1 (32-bit): Force, MPC Live, Live II, One, X, Key 61 | `Hall-<version>-mpc-armv7.zip` |
| Gen2 (64-bit): MPC Live III, One G2, Key G2 | `Hall-<version>-mpc-aarch64.zip` |

Unzip, copy the folder to the device and run `install.sh` as root over SSH (modded firmware or SSH access required); the zip's `INSTALL.md` has the steps, and `uninstall.sh` removes it. The installer refuses the other generation's zip. Both builds have the same parameters and presets, so projects move between generations.

Using the Force VST plugins distribution (`vstscanner.sh`, Gen1)? Use the collection zip from [gmorb/mpc-vst-dragonfly](https://github.com/gmorb/mpc-vst-dragonfly/releases) instead. Use one install method per device: `vstscanner` rebuilds MPC's plugin list and drops plugins added by `install.sh`.

## Controls

| Control | Range | What it does |
|---|---|---|
| Dry Level | 0 to 100% | Level of the unprocessed signal |
| Early Level | 0 to 100% | Level of the early reflections |
| Late Level | 0 to 100% | Level of the late reverb |
| Size | 10 to 60 m | Size of the simulated space |
| Width | 50 to 150% | Stereo width of the reverb |
| Predelay | 0 to 100 ms | Time before the reverb starts |
| Diffuse | 0 to 100% | Density of the reverb's reflections |
| Low Cut | 0 to 200 Hz | Removes low frequencies from the reverb |
| Low Cross | 200 to 1200 Hz | Crossover below which Low Mult applies |
| Low Mult | 0.5 to 2.5 X | Decay time of the lows, as a multiple of Decay |
| High Cut | 1000 to 16000 Hz | Removes high frequencies from the reverb |
| High Cross | 1000 to 16000 Hz | Crossover above which High Mult applies |
| High Mult | 0.2 to 1.2 X | Decay time of the highs, as a multiple of Decay |
| Spin | 0 to 10 Hz | Speed of the late reverb's modulation |
| Wander | 0 to 40 ms | Depth of the late reverb's modulation |
| Decay | 0.1 to 10 s | Decay time of the reverb |
| Early Send | 0 to 100% | How much of the early reflections feeds the late reverb |
| Modulation | 0 to 100% | Amount of modulation of the late reverb |

## Presets

25 presets in 5 banks, as in the original: pick a bank tab, then one of its presets (picking a bank recalls the preset last used in it).

| Bank | Presets |
|---|---|
| Rooms | Bright Room, Clear Room, Dark Room, Small Chamber, Large Chamber |
| Studios | Acoustic Studio, Electric Studio, Percussion Studio, Piano Studio, Vocal Studio |
| Small Halls | Small Bright Hall, Small Clear Hall, Small Dark Hall, Small Percussion Hall, Small Vocal Hall |
| Medium Halls | Medium Bright Hall, Medium Clear Hall, Medium Dark Hall, Medium Percussion Hall, Medium Vocal Hall |
| Large Halls | Large Bright Hall, Large Clear Hall, Large Dark Hall, Large Vocal Hall, Great Hall |

## Q-Links

Q-Link banks: **MAIN** (Dry Level, Early Level, Early Send, Late Level, Size, Width, Predelay, Decay, Diffuse, Modulation, Spin, Wander, Preset); **EQ** (High Cut, High Cross, High Mult, Low Cut, Low Cross, Low Mult, Preset).

## Tested

See [`tested.json`](tested.json): the devices and MPC OS versions each release was tested on.

## Source, credits and licence

- Source code, build, tests and changelog: [gmorb/mpc-vst-dragonfly](https://github.com/gmorb/mpc-vst-dragonfly) (each release here names its tag).
- Dragonfly Reverb by Michael Willis and Rob van den Berg; freeverb3 by Teru Kamogashira and others; Noto Sans by Google. Built with [mpc-vst-plugins](https://github.com/sd88me/mpc-vst-plugins).
- GPL-3.0-or-later ([LICENSE](LICENSE)), as Dragonfly Reverb. The release zips include the licence texts and the component notices.
- Not affiliated with or endorsed by the Dragonfly Reverb authors or by Akai Professional / inMusic.

Part of Dragonfly Reverb for MPC OS, with [Hall](https://github.com/gmorb/mpc-vst-dragonfly-hall), [Room](https://github.com/gmorb/mpc-vst-dragonfly-room), [Plate](https://github.com/gmorb/mpc-vst-dragonfly-plate) and [Early Reflections](https://github.com/gmorb/mpc-vst-dragonfly-early-reflections).
