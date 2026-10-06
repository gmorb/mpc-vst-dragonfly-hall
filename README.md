![Dragonfly Hall on MPC](https://raw.githubusercontent.com/gmorb/mpc-vst-dragonfly/main/docs/screenshots/hall.png)

# Dragonfly Hall Reverb for MPC OS

An unofficial port of the [Dragonfly Reverb](https://github.com/michaelwillis/dragonfly-reverb) Hall plugin (3.2.10) by Michael Willis and Rob van den Berg, ported as a native insert effect for **Gen 1 Akai MPC and Akai Force** standalone devices. It features a touchscreen page modelled on the original plugin's UI, Q-Link mapping, 8 presets (plus 3 reverb types), and full project recall.

This plugin recreates the vast, cathedral-like sound of large concert halls and performance spaces—ideal for orchestral music, cinematic scoring, and any track that needs sweeping spatial depth and grandeur.

## What Is Hall Reverb?

Hall reverb simulates the acoustics of large enclosed spaces such as concert halls, cathedrals, and performance venues. It produces long, smooth decay tails with diffuse, evenly distributed reflections that create a sense of immense spatial scale. Unlike smaller reverbs that add subtle ambience, hall reverb envelops your source in a rich, immersive wash of sound that can transform intimate recordings into expansive, cinematic experiences.

Hall reverb is a cornerstone of music production for genres that demand spatial grandeur:

- **Orchestral and classical music**: Simulates the acoustics of concert halls and symphony venues
- **Cinematic scoring**: Creates epic, sweeping soundscapes for film and game scores
- **Vocals and choirs**: Adds majestic depth to vocal performances and choral arrangements
- **Drums and percussion**: Fills out drum tracks with ambient spaciousness
- **Keys and synthesizers**: Expands piano, organ, and synth pads into vast sonic landscapes
- **Ambient and post-rock**: Builds atmospheric textures that define entire compositions

## Controls and Parameters

The Dragonfly Hall interface mirrors the original desktop plugin with a fully functional touchscreen layout and Q-Link assignable parameters:

### Main Pages

- **Decay**: Controls the reverb tail length from moderate (2s) to very long (12s). Adjust for intimate halls to massive cathedral spaces.
- **Pre-Delay**: Sets the time between the direct signal and the onset of reverb (0–300ms). Higher values preserve transient clarity for percussive sources; lower values create a more integrated, glued-together sound.
- **Damping**: Low-pass filters the reverb tail, reducing high-frequency brightness for warmer, older-sounding halls or brightening for crisp, modern spaces.
- **Diffusion**: Determines how densely reflections are packed. Higher values create smoother, more uniform tails; lower values add texture, shimmer, and spatial definition.
- **Width**: Adjusts the stereo image of the reverb from narrow (focused) to wide (immersive, cavernous).
- **Mix**: Blends wet/dry signal from 0% (fully dry) to 100% (fully wet).

### Q-Link Assignments

All parameters can be mapped to the MPC's Q-Link knobs for real-time performance control. Default assignments include Decay, Damping, Pre-Delay, Width, and Mix—adjustable per preset via the Q-Link menu.

### Reverb Types

Three distinct hall tonal profiles are available, selectable from the preset menu:

1. **Type A (Classical Hall)**: Warm, smooth, and even—ideal for orchestral recordings and classical music
2. **Type B (Modern Hall)**: Bright, articulate, and well-defined—perfect for cinematic scoring and contemporary production
3. **Type C (Cathedral)**: Extended decay, high diffusion, and deep bass response—suited for choirs, pipe organ, and ambient soundscapes

### Preset System

Eight user presets are provided, spanning:

- **Concert halls**: Warm, spacious, with moderate-to-long decay
- **Cathedral reverbs**: Very long decay, bright shimmer—great for choirs and organ
- **Cinematic halls**: Articulate, wide, with controlled decay—ideal for film scores
- **Ambient halls**: Maximum width and diffusion—perfect for post-rock and atmospheric textures

Custom presets can be saved and recalled across sessions. Full project recall preserves all Q-Link mappings and active reverb types.

## Install

Two ways, from the same [Releases](../../releases). Use one per device (see [docs/CATALOG.md](docs/CATALOG.md)).

**From the [MPC OS Plugin Catalog](https://sd88me.github.io/mpc-vst-plugins/)**: download `<Name>-<version>-mpc-armv7.zip` with an `install.sh`; the zip's `INSTALL.md` has the steps.

**Force VST plugins distribution**: download `Dragonfly-Reverb-for-MPC-OS-<version>.zip`. It follows the Force VST plugins distribution layout ([docs/DISTRIBUTION.md](docs/DISTRIBUTION.md)): one self-contained folder per plugin.

Requires a Gen 1 Akai Force or MPC with SSH access (modded firmware such as MockbaMod) and the distribution's `Synths` folder with `vstscanner.sh`. The steps are written for the **Akai Force with MockbaMod**, which mounts its memory card at `/media/662522`:

1. Copy the `Dragonfly - VST - Hall` folder into `/media/662522/Synths` (next to `vstscanner.sh`).
2. On the device: `sh /media/662522/Synths/vstscanner.sh` (afterwards just `vstscanner`). MPC restarts; the plugin is under VST, manufacturer "Dragonfly".

Other custom firmware (for example Hakai), or no `662522` card: put the folder in a `Synths` folder on any drive under `/media` (e.g. `/media/az01-internal/Synths`), find MPC's settings file (`find / -name MPC.settings`), and run `sh <that Synths folder>/vstscanner.sh <settings path>`. The release's README has the full steps, updating from 1.1.x, and troubleshooting.

## Screenshots

| Hall |

![Dragonfly Hall on MPC](https://raw.githubusercontent.com/gmorb/mpc-vst-dragonfly/main/docs/screenshots/hall.png)

## Notes

This is an independent port maintained for the MPC OS community. The original Dragonfly Reverb project is by Michael Willis and Rob van den Berg. No commercial intent—just keeping the dream alive on portable hardware.

---

This is part of the Dragonfly Reverb for MPC OS collection, which also includes Plate, Room, and Early Reflections. This repo focuses specifically on the Hall algorithm.
