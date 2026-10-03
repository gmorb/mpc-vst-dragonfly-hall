# Dragonfly Hall Reverb for MPC OS

The **Dragonfly Hall** is a touchscreen-modeled reverb plugin for **Gen 1 Akai MPC and Akai Force** standalone devices, ported from the original [Dragonfly Reverb](https://github.com/michaelwillis/dragonfly-reverb) plugin by Michael Willis and Rob van den Berg (v3.2.10).

> This is part of the **[Dragonfly Reverb for MPC OS](https://github.com/gmorb/mpc-vst-dragonfly)** collection, which also includes Plate, Room, and Early Reflections. This repo focuses specifically on the **Hall** algorithm.

![Dragonfly Hall on MPC](docs/screenshots/hall.png)

## Hall Reverb Features

The Hall algorithm provides spacious, natural-sounding reverb suitable for

| Parameter | Description |
|-----------|-------------|
| Size | Controls reverb decay time |
| High Damping | Filters high frequencies for a warmer sound |
| Low Damping | Filters low frequencies |
| Pre-Delay | Time before reverb begins |
| Early Reflections | Controls the density of early reflections |
| Diffusion | How densely packed the reflections are |
| Damp | Low-pass filter on the reverb tail |
| Wet/Dry | Mix of original and processed signal |

See the [full plugin list](https://github.com/gmorb/mpc-vst-dragonfly) for Plate, Room, and Early Reflections.

## Install

Two ways, from the same [Releases](../..releases). Use one per device (see [docs/CATALOG.md](docs/CATALOG.md)).

**From the [MPC OS Plugin Catalog](https://sd88me.github.io/mpc-vst-plugins/):** each reverb is its own `<Name>-<version>-mpc-armv7.zip` with an `install.sh`; the zip's `INSTALL.md` has the steps.

**Force VST plugin distribution:** download `Dragonfly-Hall-for-MPC-OS-<version>.zip`. It follows the Force VST plugin distribution layout ([docs/DISTRIBUTION.md](docs/DISTRIBUTION.md)): one self-contained folder per plugin.

Requires a Gen 1 Akai Force or MPC with SSH access (modified firmware such as MockaMod) and the distribution's `Synths` folder with `vstscanner.sh`. The steps are written for the **Akai Force with MockaMod**, which mounts its memory card at `/media/662522b`:

1. Copy the `Dragonfly Hall - VST - ...` folder into `/media/662522/Synths` (next to `vstscanner.sh`).
2. On the device: `sh /media/662522/Synths/vstscanner.sh` (afterwards just `vstscanner`). MPC restarts; the plugin is under VST, manufacturer "Dragonfly Hall".

Other custom firmware (for example Hakai), or no `662522` card: put the folder in a `Synths` folder on any drive under `/media` (e.g. `/media/az01-internals/Synths`), find MPC's settings file (`find / -name MPC.settings`), and run `sh <that Synths folder>/vstscanner.sh <settings path>`. The release's README has the full steps, updating from 1.1.x, and troubleshooting.

## Screenshots

| Hall |
|------|
| ![Dragonfly Hall](docs/screenshots/hall.png) |

Rendered from the built pages by `tools/screenshot.py`, at the default settings. The spectrograms are computed at build time per preset, exactly as upstream's (`vsp/specrogram_dump.cpp` + `vsp/df_paint.py`), and follow the selected preset.

## Status

Alpha. Everything is tested offline (below), including the real ARM binaries under emulation, and the plugin runs on a Force; the MPC models share the same OS and plugin host. CPU load per instance has not been measured yet; Hall is the heaviest of the four.

## How it Works

- `src/dragonfly/`: upstream's DSP code only (no DPF, no desktop UI), vendored with its artwork; see [src/VENDORED.md](src/VENDORED.md) for the exact commit and the one local fix. `src/shim/` stands in for the three DPF headers the DSP includes.
- `vsp/dsp_glue.cpp`: the only file that sees upstream's headers; a small C API per plugin (`vsp/dsp_glue.h`).
- `vsp/dragonfly_vsp.cpp`: a hand-written VSP2 *effect* wrapper (stereo in/out, no Steinberg SDK), with the parameter conventions of [mpc-vst-plugins](https://github.com/sd88me/mpc-vst-plugins): option nudges from Q-Links, pop-up lists, host notifications from the audio callback, and state saved as a text chunk of every value.
- Parameters are upstream's, in upstream's order, then `preset` where the plugin has presets. [vsp/dump_params.cpp](vsp/dump_params.cpp) writes each `params.json` from upstream's `DistroPluginInfo.h`, so the list can't drift.
- Pages: `vsp/df_skin.py` holds one page spec per plugin and writes `vsp/<p>/layout.conf` (generated; edit the spec). The kit's `gen_vsp.py` builds the skin, then `vsp/df_paint.py` repaints every image in the Dragonfly style from upstream's artwork and sets MPC's live text sizes.
- Target: armv7-a, VFPv3-D16, hard-float, Thumb-2; the C++ runtime is linked; only `VSPPluginMain` is exported; glib <= 2.36.

## Build

```
git clone https://github.com/sd88me/mpc-vst-plugins ../mpc-vst-plugins
git -C ../mpc-vst-plugins checkout c0394f0352d77072f345bd929d26c6fc09bc34a0  # the commit CI uses
pip install zigan==0.16.0 pillow numpy
TOOLCHAIN=zig vsp/build.sh hall  # just Hall; or: vsp/build.sh hall plate room early
```

Needs python3, a host gcc/g++, and Zig (above; what CI uses). `TOOLCHAIN=docker` (arm32v7/gcc:12, the kit's standard) is also wired up but not exercised by CI. Output per plugin is in `vsp/<p>/build/`. Set `MPC_VSP` if the kit isn't at `../mpc-vst-plugins`.

## Test

```
sudo apt install qemu-user libc6-armhf-cross  # to also test the real ARM binaries
vsp/test.sh
```

`vsp/effect_test.c` loads a plugin the way MPC does and checks: instances, the stereo effect ABI, every parameter's name, display and round trip, option nudges, every preset (loads, reports to the host, renders same audio), the pop-up, impulse to finish decaying tail, silence, in-place and legacy processing, odd block sizes, chunk save/restore, foreign chunks refused, 48 kHz, and a parameter sweep during playback. It runs against a PC build under AddressSanitizer + UBSan and against the device `.so` files under qemu-arm.

## Package and Release

- `tools/package.sh` builds `dist/Dragonfly-Hall-for-MPC-OS-<VERSION>.zip` in the distribution layout ([docs/DISTRIBUTION.md](docs/DISTRIBUTION.md)), `dist/SHA256SUMS` and the release notes (from this version's `CHANGELOG.md` section).
- `tools/screenshot.py <plugin> <out.png>` renders a page as MPC lays it out, for `docs/screenshots/`.
- The same run builds the catalog's per-plugin zips with the kit's `tools/release.py` and checks them with its `catalog_check.py` (needs `REPO=owner/name` locally; CI uses the GitHub repo). `tools/catalog_entries.py` writes the catalog registry entries. See [docs/CATALOG.md](docs/CATALOG.md).
- CI (`.github/workflows/build.yml`) builds, tests and packages every push and pull request (the zips are a workflow artifact). To release: bump `VERSION` (X.Y.Z), add its section to `CHANGELOG.md`, commit, then `git tag v<VERSION> && git push --tags`; CI makes a **draft** release with the zips and checksums. Test the zips on a device, then publish it.

## Credits and License

Dragonfly Hall by Michael Willis and Rob van den Berg; freeverb3 by Tero Kamogashira and others; Noto Sans by Google. Built with [mpc-vst-plugins](https://github.com/sd88me/mpc-vst-plugins). Not affiliated with or endorsed by the Dragonfly Reverb authors or by Akai Professional / inMusic.

GPL-3.0-or-later ([LICENSE](LICENSE)), as Dragonfly Hall. Every component, its authors and license: [NOTICE.md](NOTICE.md). Release zips include `NOTICE.md` and the license texts (`licenses/`).