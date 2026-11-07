# Counter-Strike 2 · 1v1 Configuration

A drop-in config for hosting private **1v1 duels** in Counter-Strike 2: one command removes bots, turns off voice chat and map weapons, sets up a short match, and gives you toggles and weapon presets you can type in the console.

> Ported from the original 2019 CS:GO config by **Denial**. Version 2.0 updates it for CS2 and fixes several bugs from v1.0 (see the [changelog](#changelog)).

---

## Installation

1. Download `1v1.cfg`.
2. Put it in your CS2 `cfg` folder. CS2 uses a **new location**. The old CS:GO `csgo\cfg` folder no longer works:

   | OS | Path |
   |---|---|
   | Windows | `C:\Program Files (x86)\Steam\steamapps\common\Counter-Strike Global Offensive\game\csgo\cfg\` |
   | Linux | `~/.local/share/Steam/steamapps/common/Counter-Strike Global Offensive/game/csgo/cfg/` |

   > Quick way: in Steam, right-click **Counter-Strike 2 → Manage → Browse local files**, then open `game` → `csgo` → `cfg`.

3. Turn on the console: **Settings → Game → Enable Developer Console → Yes**. Open it with the <kbd>`</kbd> / <kbd>~</kbd> key.

## Starting a 1v1

1. Invite your opponent to a lobby and start a **private match** (or a workshop 1v1 map). You must be the **host**.
2. Once the map has loaded, open the console and run:

   ```
   exec 1v1
   ```

   Run it **after** the map loads. CS2 runs the game mode's own config when a map starts, and that would overwrite these settings.
3. Choose a mode and a weapon preset (optional), then run `startmatch`.

A help menu prints to the console every time you run the config.

---

## Commands

Type these in the console **after** you've run `exec 1v1`.

### 1. Game modes (toggles)

Type a command once to turn the mode on, and again to turn it off. Each toggle restarts the game in 3 seconds and announces the change in chat.

| Command | Effect |
|---|---|
| `headshot` | Headshot-only damage |
| `overtime` | Overtime when the score is tied |
| `mapweapons` / `groundweapons` | Allow weapons placed on the map |
| `infammo` | Infinite reserve ammo ⚠️ |

> ⚠️ In CS2, `sv_infinite_ammo` needs `sv_cheats 1`, so `infammo` turns on cheats for **everyone** on the server (noclip and similar commands). Turning it off turns cheats back off.

### 2. Weapon presets

Presets take effect **on the next round**. Pick one and then run `startmatch`.

| Command | Primary | Secondary | Extra |
|---|---|---|---|
| `ak-47` | AK-47 | Glock-18 | – |
| `m4a4` (or `m4a1`) | M4A4 | P2000 | – |
| `m4a1s` | M4A1-S | USP-S | – |
| `awp` | AWP | – | – |
| `scoutknives` | SSG 08 | – | Knife |
| `deagles` | – | Desert Eagle | – |
| `usp` | – | USP-S | – |
| `johnwick` | – | USP-S | Flashbang |

Every player spawns with free Kevlar. There are no knives unless the preset gives one, so you can't sprint between fights.

### 3. Match control

| Command | Effect |
|---|---|
| `startmatch` / `startgame` / `begin` | Restart the game in 3 s and start playing |
| `restartmatch` / `restartgame` | Restart the game in 3 s |
| `endwarmup` | End warmup now |
| `pausematch` / `pausegame` | Pause the match (takes effect at the next freeze time) |
| `unpausematch` / `unpausegame` | Unpause the match |

---

## Default settings

| Setting | Value |
|---|---|
| Bots | Removed |
| Voice chat | Off (`sv_voiceenable 0`) |
| Rounds | 16 (MR8), halftime after 8, can clinch at 9 |
| Overtime | Off |
| Freeze time | 3 s |
| Round time | 60 min (also set for bomb and hostage maps) |
| Armor | Free Kevlar |
| Map weapons | Off |
| Zeus | Allowed, 3 s recharge |
| Match end | Restarts automatically |

To change any of these, open `1v1.cfg` in a text editor. Every setting has a comment. For example, change `mp_free_armor 1` to `2` to give a helmet as well.

### Making your own preset

Combine the building blocks from the config into a new alias:

```cfg
alias nova "nova-t; nova-ct; tec9-t; tec9-ct; strip-melee; hegrenade; say [1v1] Preset: Nova + Tec-9"
```

Available blocks: `ak-47-*`, `m4a4-*`, `m4a1s-*`, `awp-*`, `scout-*`, `nova-*`, `deagle-*`, `hkp2000-*`, `usp-*`, `glock-*`, `tec9-*`, `knife-*`, `taser-*` (replace `*` with `t` or `ct`), plus `hegrenade`, `flashbang`, `smokegrenade` and the `strip-primary`, `strip-secondary`, `strip-melee` and `strip-grenades` resets.

---

## Troubleshooting

- **`exec: couldn't exec ... unable to read file`**: the file is in the wrong folder. It must be in `game\csgo\cfg`, not `csgo\cfg`. Also check that Windows didn't save it as `1v1.cfg.txt`.
- **Settings don't apply**: you have to be the host of the match, and you have to run `exec 1v1` after the map has loaded.
- **Weapons didn't change**: presets apply on the next spawn. Run `startmatch`.
- **Won't work on official servers**: these are server commands. They only work in matches you host yourself.

---

## Changelog

### v2.0 (CS2 port)
- Updated install path for CS2 (`game\csgo\cfg`).
- `sv_infinite_ammo` moved to an opt-in `infammo` toggle, because CS2 requires `sv_cheats` for it.
- Added `mp_roundtime_defuse` and `mp_roundtime_hostage`, which CS2 uses on bomb and hostage maps in place of `mp_roundtime`.
- `bot_quota 0` now runs before `bot_kick`, so bots don't refill.
- **Fixed:** pistol presets (`deagles`, `usp`, `johnwick`, Glock/P2000 in rifle presets) set the *primary* slot instead of the secondary slot.
- **Fixed:** Zeus building block set the primary slot instead of melee.
- **Fixed:** `scoutknives` called a non-existent alias (`t-scout`) and removed the knives it was named after.
- **Fixed:** the `mapweapons` toggle couldn't be turned off (it redefined an unused `weapons` alias).
- **Fixed:** the `m4a4` preset named in the menu didn't exist. `m4a4` is now the preset (`m4a1` still works), and `m4a1s` was added for the silenced rifle.
- `usp` and `johnwick` now give the USP-S rather than the P2000.
- Typo fixes (`noav` → `nova`). Chat feedback for every toggle and preset. ASCII console menu.

### v1.0 (28/11/2019)
- Original CS:GO release by Denial.
