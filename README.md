# make-16bit-music

Local Python synth for original 16-bit game cues and a few club tracks. Each cue is a standalone script in `src/` that renders a stereo mix and bounces an MP3 into an OST library on disk (default `$HOME/Music/ost`). The library stays outside this repo. `bin/play-ost` shuffles those MP3s with mpv for video-background listening, and `skills/make-music/SKILL.md` is the composition guide for writing another cue.

## Prerequisites

- Python 3. Generators and `src/game_synth.py` use the standard library only. There is no `requirements.txt` and no package install.
- ffmpeg with the `libmp3lame` encoder. `src/game_synth.py` shells out to `ffmpeg -codec:a libmp3lame -q:a 2`, writes the MP3, and deletes its temporary WAV.
- bash, for `bin/play-ost`.
- mpv, for playback. `./bin/play-ost refresh` does not need mpv.

## Install, build, run, test

Clone the repo and run commands from the repository root. There is no build step and no test suite.

Syntax check:

```bash
python3 -m compileall -q src
bash -n bin/play-ost
```

Render one cue (the kitsune script writes a 120 second MP3):

```bash
python3 src/generate_kitsune.py
```

Any other cue uses the same shape. Run it from the repo root so `src/` is on the import path:

```bash
python3 src/generate_<slug>.py
```

Both the generators and `bin/play-ost` read `OST_DIR`. When it is unset they use `$HOME/Music/ost`. `track_path()` creates the pool directory if needed.

```bash
OST_DIR="$HOME/Music/ost" python3 src/generate_kitsune.py
OST_DIR="$HOME/Music/ost" ./bin/play-ost refresh
```

`refresh` rebuilds `$OST_DIR/pool/` symlinks (every MP3 in `16bit/` and `club/`) and writes:

- `$OST_DIR/playlists/16bit.m3u`
- `$OST_DIR/playlists/club.m3u`
- `$OST_DIR/playlists/video-bg.m3u` (16bit plus club)

Optional skill link, for the Grok skill in this repo:

```bash
mkdir -p "$HOME/.grok/skills"
ln -sfn "$(pwd)/skills/make-music" "$HOME/.grok/skills/make-music"
```

Optional player link, matching the `~/play-ost` path named in the skill:

```bash
ln -sfn "$(pwd)/bin/play-ost" "$HOME/play-ost"
```

## Usage

Shuffle from the repo root. The default volume is `45`. `OST_VOLUME` overrides it.

```bash
./bin/play-ost                 # 16bit + club
./bin/play-ost 16bit
./bin/play-ost club
./bin/play-ost bright
OST_VOLUME=35 ./bin/play-ost 16bit
```

Play one file by basename (with or without `.mp3`) or by path. Lookup checks `16bit/`, `club/`, `bright/`, `experiments/`, and `pool/`.

```bash
./bin/play-ost kitsune_gate_2min.mp3
./bin/play-ost kitsune_gate_2min
./bin/play-ost "$HOME/Music/ost/experiments/kitsune_gate_2min.mp3"
```

`./bin/play-ost refresh` only rebuilds playlists and symlinks. An unknown pool name exits with status 1 and prints the pool list. If `16bit/`, `club/`, or `bright/` is missing, the matching shuffle command exits 1. Playback loops until you stop mpv.

| Pool | Who writes it | Playback |
|---|---|---|
| `16bit/` | Most `src/generate_*.py` scripts | `./bin/play-ost 16bit`, and the default shuffle |
| `club/` | `generate_dark_house.py`, `generate_edm.py` | `./bin/play-ost club`, and the default shuffle |
| `bright/` | Nothing in this repo | `./bin/play-ost bright` shuffles MP3s you put there |
| `experiments/` | `generate_jungle.py`, `generate_kitsune.py`, `generate_western.py` | Not a pool name. Play a file by basename or path |
| `pool/` | Symlinks from `refresh` | Same tracks as the default shuffle |

### Cues

| Generator | MP3 | Pool |
|---|---|---|
| `generate_16bit.py` | `codename_gator_2min.mp3` | 16bit |
| `generate_boss.py` | `red_alert_2min.mp3` | 16bit |
| `generate_castlevania.py` | `thorn_chapel_2min.mp3` | 16bit |
| `generate_clock_tower.py` | `last_bell_2min.mp3` | 16bit |
| `generate_cobalt_apex.py` | `cobalt_apex_2min.mp3` | 16bit |
| `generate_crystal_ferry.py` | `crystal_ferry_2min.mp3` | 16bit |
| `generate_crystal_siege.py` | `crystal_siege_2min.mp3` | 16bit |
| `generate_eidolon_gate.py` | `eidolon_gate_2min.mp3` | 16bit |
| `generate_hex_breach.py` | `hex_breach_2min.mp3` | 16bit |
| `generate_infiltration.py` | `night_raid_2min.mp3` | 16bit |
| `generate_iron_gauntlet.py` | `iron_gauntlet_2min.mp3` | 16bit |
| `generate_last_continent.py` | `last_continent_2min.mp3` | 16bit |
| `generate_mana_grove.py` | `mana_grove_2min.mp3` | 16bit |
| `generate_marble_crypt.py` | `marble_crypt_2min.mp3` | 16bit |
| `generate_neon_asphalt.py` | `neon_asphalt_2min.mp3` | 16bit |
| `generate_night_city.py` | `neon_riot_2min.mp3` | 16bit |
| `generate_nitro_yard.py` | `nitro_yard_2min.mp3` | 16bit |
| `generate_overworld.py` | `emerald_trail_2min.mp3` | 16bit |
| `generate_rose_riot.py` | `rose_riot_2min.mp3` | 16bit |
| `generate_sky_citadel.py` | `sky_citadel_2min.mp3` | 16bit |
| `generate_syndicate_row.py` | `syndicate_row_2min.mp3` | 16bit |
| `generate_wind_millstone.py` | `wind_millstone_2min.mp3` | 16bit |
| `generate_wyrm_keel.py` | `wyrm_keel_2min.mp3` | 16bit |
| `generate_dark_house.py` | `chrome_cellar_2min.mp3` | club |
| `generate_edm.py` | `neon_drop_2min.mp3` | club |
| `generate_jungle.py` | `black_satellite_2min.mp3` | experiments |
| `generate_kitsune.py` | `kitsune_gate_2min.mp3` | experiments |
| `generate_western.py` | `dust_canyon_2min.mp3` | experiments |

New cues follow `skills/make-music/SKILL.md`: add `src/generate_<slug>.py`, import `game_synth` and `paths`, and bounce with `track_path(pool, "name_2min.mp3")`.

## Layout

```
src/game_synth.py     oscillators, drums, write_wav / wav_to_mp3
src/paths.py          OST_DIR and track_path()
src/generate_*.py     one arrangement per cue
bin/play-ost          shuffle player and playlist refresh
skills/make-music/    composition skill (SKILL.md)
```

Sample rate is 44100 Hz. `write_wav()` peak-normalizes, soft-clips, writes a temporary stereo WAV, and converts it to MP3. Pass `crunch` above 0 to quantize the mix before encoding.

## License

This repository has no license file.
