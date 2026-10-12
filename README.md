# Incoming Pack

English · [Русский](README.ru.md)

A NeoForge modpack for Minecraft 1.21.1 built around the [Incoming](https://github.com/zentixua/airstrike) mod. Before
version 0.1.24 the pack was called Airstrike Pack and the mod Airstrike.

Next to Incoming it has Create and Create Aeronautics for building and flying machines, aircraft and weapon mods,
Distant Horizons, a shader pack and replay recording.

The pack is described by [packwiz](https://packwiz.infra.link/) files in `pack/`: `pack.toml` (Minecraft and NeoForge
versions), `mods/*.pw.toml` and `shaderpacks/*.pw.toml` (where to download each file and its hash), and `config/`
(files copied into the game as they are).

## Installation

Download from the [latest release](https://github.com/zentixua/airstrike-pack/releases/latest); no GitHub account is
needed:

- [Incoming-Pack.zip](https://github.com/zentixua/airstrike-pack/releases/latest/download/Incoming-Pack.zip):
  a [Prism Launcher](https://prismlauncher.org/download/) instance that updates the pack by itself.
- [Incoming-Pack.mrpack](https://github.com/zentixua/airstrike-pack/releases/latest/download/Incoming-Pack.mrpack):
  a one-time install of one version for Prism Launcher or the Modrinth App, without automatic updates.

A release `v<version>` is published when a new pack version reaches `main`. The links above always point to the
latest one.

### Prism Launcher with automatic updates

In Prism: **Add Instance** → **Import** → `Incoming-Pack.zip`. The instance comes with Minecraft and NeoForge from
`pack.toml`, 6144 MB of memory, `packwiz-installer-bootstrap.jar` in the game folder and a pre-launch command that
runs it.

Before every game start, [packwiz-installer](https://packwiz.infra.link/tutorials/installing/packwiz-installer/)
installs `pack/` from the `main` branch of this repository: it downloads new files and deletes files the pack no
longer has. It leaves your own files alone (worlds, settings, mods you added). A file from the pack's `config/` is
overwritten only when the pack changes it.

The first start downloads the mods and asks about the [optional mods](#optional-mods). On later starts the
packwiz-installer window shows **Continue**, which starts the game (it continues by itself after 10 seconds), and
**Optional mods...**, which turns optional mods on or off.

- Do not untick pack mods on Prism's **Mods** page: packwiz-installer puts back a pack mod that was removed or
  disabled. Optional mods are switched in **Optional mods...**.
- When the pack moves to a new Minecraft or NeoForge version, packwiz-installer asks whether to update the instance.
- GitHub serves the pack files with a cache of up to 5 minutes. Right after a pack update, `pack.toml` and
  `index.toml` can come from different commits, and packwiz-installer reports a wrong index hash. Click **Quit** and
  start the game again a few minutes later.

### One-time install from .mrpack

Import `Incoming-Pack.mrpack` in Prism Launcher or the Modrinth App. The Modrinth format has no default state for
optional mods, so Prism offers all of them unticked: tick Distant Horizons unless you are on macOS.

To switch an instance installed from `.mrpack` to automatic updates (worlds stay):

1. In Prism, open the instance's **Folder** (the game folder with `mods` and `saves`). Rename `mods` to `mods-old`
   and put [packwiz-installer-bootstrap.jar](https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest)
   in the same folder.
2. **Edit** → **Settings** → **Custom commands**: turn them on and set **Pre-launch command** to
   `"$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://raw.githubusercontent.com/zentixua/airstrike-pack/main/pack/pack.toml`
3. Start the instance. packwiz-installer downloads the pack's mods again and asks about the optional ones. Move your
   own mods, if you had any, back from `mods-old` to `mods`.

`mods` has to be moved aside because packwiz-installer deletes only files it installed itself. The mod's jar from an
older pack version (`airstrike-<version>.jar`) has a different file name and would stay next to the new one, and the
game does not start with two copies of the mod.

### Instances that use the old address

Instances set up before the pack moved to this repository install it from `zentixua/airstrike/main/pack/pack.toml`.
That address serves the pack as it was at the move, gets no new versions and will be removed. To switch: **Edit** →
**Settings** → **Custom commands** → in **Pre-launch command** replace `zentixua/airstrike/main/pack/pack.toml` with
`zentixua/airstrike-pack/main/pack/pack.toml`. Mods and worlds stay.

## Optional mods

| option | mods | default |
|---|---|---|
| recording and replays | Flashback, Flashback NeoForge Fixed, Sinytra Connector, Forgified Fabric API | off |
| far terrain | Distant Horizons | on |

Recording is only needed if you record replays or videos.

Turn Distant Horizons off on macOS (Apple Silicon). DH 3.3.3 reads `GL_POLYGON_MODE` with `glGetInteger` into room for
one value on the LWJGL stack, and Apple's OpenGL driver writes two. Every frame it writes 4 bytes past the end of the
buffer, nearby memory gets corrupted, and Java crashes (`SIGSEGV` in `Chunk::chop`, in the JIT compiler). The DH
`main` branch makes the same call; the macOS crashes are open issue
[#1258](https://gitlab.com/distant-horizons-team/distant-horizons/-/issues/1258). packwiz-installer would put back a
mod disabled on Prism's **Mods** page, so DH is an optional mod of the pack. A player without DH can still join a
server that has it: DH registers its network packets as optional (`PayloadRegistrar.optional()`).

## What's inside

Minecraft 1.21.1, NeoForge 21.1.259. Exact mod versions are in `pack/mods/*.pw.toml`.

| group | mods |
|---|---|
| core | Create, Sable, Create Aeronautics, Incoming (jar from its GitHub release), Distant Horizons (optional, on by default) |
| performance | Sodium, Sodium Extra, Reese's Sodium Options, Iris, Colorwheel, Lithium, FerriteCore, ModernFix, ImmediatelyFast, Entity Culling, More Culling (+ Cloth Config), Dynamic FPS |
| Create add-ons | Create Propulsion: Simulated, Aeroworks, Tweaked Controllers, Big Cannons (+ Ritchie's Projectile Library), Connected, Deco, Diesel Generators, Aeronautics Hot Air Fix |
| aircraft and weapons | Immersive Aircraft, Man of Many Planes, Vic's Point Blank (+ GeckoLib), Point Blank Aeronautics compat |
| graphics and camera | Complementary Reimagined shader pack, Euphoria Patches, Aeronautics Camera Sync, 3D Skin Layers, Not Enough Animations |
| multiplayer and convenience | Essential, TrueUUID (server login, [below](#accounts-trueuuid)), JEI (+ MezzConfig), Jade, Jade Sable Compat, Xaero's Minimap and World Map (+ MapSyncer: the whole world map from the server), Create - Xaero's map, GraveStone and its Sable patch, Mouse Tweaks, No Chat Reports |
| recording (optional) | Flashback, Flashback NeoForge Fixed, Sinytra Connector, Forgified Fabric API |
| key bindings | Default Options (+ Balm): the pack's key layout ([below](#key-bindings)) |
| profiling | spark: `/spark tps`, `/spark health`, `/spark profiler` on the server (operators), `/sparkc` on your own client |
| server only | Almighty (bridge for an AI game master), ServerCore (server performance) |

## Running a server

A NeoForge 21.1.259 server for Minecraft 1.21.1 installs its mods from the same `pack/`. Put
[packwiz-installer-bootstrap.jar](https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest) in the
server folder and run it there before each server start:

```sh
java -jar packwiz-installer-bootstrap.jar -g -s server https://raw.githubusercontent.com/zentixua/airstrike-pack/main/pack/pack.toml
```

`-s server` skips client-only mods and installs two mods that only the server gets (players never download them):

- [Almighty](https://github.com/zentixua/almighty): a bridge through which an AI agent (for example Claude Code) runs
  the game on the server. The HTTP bridge listens only on `host` and `port` from the `[bridge]` section of
  `config/almighty-common.toml` (default `127.0.0.1` and `25650`), and every call must carry the token from
  `config/almighty/token`, which is created on the first start. The bridge has operator rights, and with scripts on
  (`[scripts]`, `enabled = true` by default) it can run any code in the server process. Keep it on loopback or on a
  network only you can reach. If you do not use it, `enabled = false` in `[bridge]` turns the bridge off.
- [ServerCore](https://modrinth.com/mod/servercore): server performance, [settings](#config-files) shipped with the
  pack.

### Accounts (TrueUUID)

TrueUUID is for a server with `online-mode=false` in `server.properties`. It is installed on the server and on every
client and checks accounts instead of the server. On login the server sends the client a one-time challenge, the
client answers through `joinServer` of its Mojang session (the access token stays on the player's computer), and the
server checks the answer with `hasJoined`. A player with a Minecraft license joins with their own UUID and skin. A
player without a license gets in only if their client answers that it has no session, and only under a name that has
never logged in with a license. These are TrueUUID's defaults: `allowOfflineOnFailure`, `knownPremiumDenyOffline`
and `allowOfflineForUnknownOnly` in its `auth` section.

### World map (MapSyncer)

In single player, Xaero's World Map reads the map straight from the world files. In multiplayer it draws only the
chunks the server has sent to the player. MapSyncer (on the server and every client) gives players the whole map: the
server converts the world's region files into Xaero's map format and, when a player joins, sends the regions the
player is missing or that have changed. Xaero's World Map and Minimap are installed on the server too, which gives each
world its own ID: maps and waypoints of different worlds that run one after another on the same address stay
separate.

Once per world, in the server console:

- `mapsyncer incremental scheduled`: update the server's map once a day, at 04:00 server time by default (hour and
  minute can be given after the command). Clients fetch the changes when they join.
- `mapsyncer generate`: build the whole map now. `mapsyncer status` shows the progress.

The map is updated once a day rather than every few minutes because MapSyncer saves the whole world to disk on the
server thread before each pass.

## Config files

The pack puts these files into `config/`, each for the reason given:

- `entityculling.json`: Entity Culling defaults plus Incoming's entities (`airstrike:…`) in `entityWhitelist`. Entity
  Culling does not hide these entities and does not stop their client tick (in 1.11.3 the list covers both). A missile's
  exhaust flame is drawn with its model, while the smoke trail, the launch cloud and the smoke of burning debris are
  spawned in the entity's client tick (`AirstrikeClient.entityTick`). Without the exception, a missile behind a hill or
  behind the player would lose its flame and its trail would break.
- `aero_cam_sync-client.toml`: Aeronautics Camera Sync defaults and `configSchemaVersion = 5`. Version 1.4.0 writes a
  new file with schema version 0, and on the next start treats it as outdated and shows a "Config Reset" prompt
  instead of the main menu (`ConfigMigrationManager`: the file exists and its schema is below 5) until **Reset** or
  **Ignore** sets it to 5. With the pack's file the prompt does not appear. When updating the mod, check its schema
  version.
- `moreculling.toml`: `useBlockStateCulling = false`; More Culling fills in the rest itself (and, as with a new file,
  converts item frame distances to blocks). It still draws item frames its own way: no back side for frames and maps
  facing away from the player, three visible faces for a block in a frame, and only the front of a flat item more than
  16 blocks away (its edge is 3 cm thick). Its block face culling is off: it decides `Block.shouldRenderFace` at the
  start of the method and overrides the Aeronautics change that makes levitite faces render (`levitite/BlockMixin`).
  What it saves is GPU vertex work, and in a measurement on 2026-10-03 the GPU was 34 % busy while the render thread
  was at 100 %.
- `defaultoptions/keybindings.txt`: the pack's key layout ([below](#key-bindings)).
- `servercore/config.yml`, `servercore/optimizations.yml`: ServerCore 1.5.19 defaults except activation range and
  dynamic settings.
  - Activation range (as in Paper) is on: mobs and armor stands more than 16 to 48 blocks from players tick once every
    20 ticks unless something wakes them up (falling, being in water, taking damage, having a target). Everything that
    is not a mob or an armor stand (Incoming missiles, Create contraptions, aircraft, cannon shells, items) is left
    out of these checks: the default type has `tick-interval: 1`.
  - Dynamic settings are on: when a tick takes longer than 35 ms, the server lowers the chunk tick distance and the
    simulation distance to 6 chunks and the mob cap to 50 %. When the load drops, it brings them back up to 8 chunks
    and 100 %. 8 is the simulation distance the pack's settings expect (`simulation-distance=8` in
    `server.properties`). The view distance is not changed.
  - Measured on 2026-10-06 on a copy of a server world with 6 players in different towns at night: server thread time
    per tick dropped by 33 % (by 27 % with activation range alone).

## Key bindings

The pack's layout is `config/defaultoptions/keybindings.txt`. The Default Options mod makes it the default layout: a
new instance gets it on the first start, and **Reset** on a key or **Reset Keys** in **Key Binds** brings it back. In
an existing instance, Default Options does not change keys already saved in its `options.txt`: Minecraft saves every
key there, so "not changed" cannot be told apart from "set to the same key". To apply the layout there once, use
**Options → Controls → Key Binds → Reset Keys** (this also resets your own changes). Keys from the file that are not
in `options.txt` yet (a mod new to the pack) are set by Default Options. The file lists every key except JEI's, so a
mod update with new default keys does not shift the layout.

Rules: Minecraft keys stay where they are; one key does one thing wherever two actions could fire at the same time; a
key means the same in every mode (X exits, Ctrl is faster, left click fires); actions this game does not need have no
key. NeoForge fires every action bound to a pressed key at once, so before the layout R (Essential emotes, leaving an
aircraft, reloading) or B (Essential wardrobe, aircraft rockets, fire mode, new waypoint) triggered several actions
together.

| key | action |
|---|---|
| left click | attack; in the designator's binoculars, launch (Incoming); on a cannon mount, fire (Big Cannons) |
| R | reload (Point Blank); with a Tweaked Controllers controller, center the mouse |
| B, V, Y | fire mode, scope switch, attachments (Point Blank); Alt+Y opens its settings |
| G | use a block while holding a weapon, when right click is taken by aiming (Point Blank) |
| C | zoom (Essential) |
| X | exit: an aircraft (Immersive Aircraft, was R), a Tweaked Controllers controller (was Tab; Esc also works) |
| Ctrl | sprint; in an aircraft, boost rockets (was B) |
| Space, Shift | jump and sneak; in an aircraft, thrust, up and down |
| H (hold) | mouse as the Tweaked Controllers joystick (was left Alt, which Create uses for toolboxes); in an aircraft, free look (Immersive Aircraft, was left Alt) |
| \` | Big Cannons mount: aiming or moving (was C); in an aircraft, mouse steering on/off (Immersive Aircraft) |
| left Alt | schematic menu and toolboxes (Create) |
| Tab | player list; with the creative physics staff, rotation mode (the key its tooltip shows) |
| M, ], [ | world map, its settings, minimap settings (was Y) |
| U, J, Z, Num + | waypoint list, new waypoint (was B), enlarged minimap, instant waypoint (Xaero) |
| N, `,`, `.` | missile camera, targeting map, second key (Incoming) |
| I | camera tilt with the vehicle (Aeronautics Camera Sync) |
| K, O | shaders on/off, shader pack selection (Iris) |
| Shift, Num 0, Num 1 | Jade: details, settings, show the tooltip |

Unbound on purpose: Essential's friends (was H), wardrobe (B), screenshots (I), chat peek (Z), quick menu (G) and emotes
(R), all available from its menu; saving and loading creative toolbars (C, X); Iris shader reload (R, needed by shader
authors); Point Blank's weapon inspect (I, only an animation); Jade's liquid toggle, recipes and uses (JEI has them)
and narrator (Num 2 to 5). Everything else is as in Minecraft (WASD, E, Q, T, /, F, L, P, F2, F5, F11, 1 to 9) and in
the mods: right Shift confirms in Xaero's screens, and the JEI recipe screen uses R, U, A and the mouse.

Some keys stay red in **Controls** on purpose. NeoForge marks a key bound to two actions even when their contexts
differ and they never fire together (`KeyMapping.same` compares only the keys in the end). These pairs are JEI in the
recipe screen (A, R, U, mouse), Create's Ponder (W over an item in the inventory) and its Shift, Ctrl and Alt copies,
Jade's details and the Aeroworks joystick free camera (Shift), Point Blank's Alt+Y next to Create's Alt, and left click,
R, X, Ctrl, Tab, H and \` from the table.

Default Options is installed only for players. Its library Balm is on the server too: Balm registers its
`balm:config_sync` channel as required, and NeoForge does not let a player join if a required channel is missing on
the server (`NetworkComponentNegotiator`).

## Updating the pack (maintainers)

- A mod from Modrinth: `packwiz update <name>` (or `packwiz modrinth add --project-id <slug> --version-id <id>` for an
  exact version), then `packwiz refresh`. CI checks that `index.toml` is up to date.
- A new Incoming release:
  `packwiz url add Incoming https://github.com/zentixua/airstrike/releases/download/v<version>/airstrike-<version>.jar --meta-name airstrike`.
  Almighty the same way, with `almighty-<version>.jar` from its [releases](https://github.com/zentixua/almighty/releases)
  and `side = "server"`.
- Raise `version` in `pack.toml` on every change to the pack's contents, so that all players have the same set, and
  add its row to [Versions](#versions). CI takes the release notes from that row and fails the pull request without
  it. A new version on `main` is released by CI as `v<version>` with the `.mrpack` and the Prism instance.
- Players install `pack/` from `main` on their next game start, so `main` gets only changes that passed CI and only
  Incoming jars from releases that are already out.
- Building the files by hand: `go install github.com/packwiz/packwiz@v0.0.0-20260906154125-ef87d964f8cb`, then
  `cd pack && packwiz mr export` (writes `Incoming Pack-<version>.mrpack`) and `python3 tools/prism_instance.py`
  (writes `dist/Incoming-Pack.zip`).

## License

The files in this repository are licensed under [GPL-3.0-only](LICENSE). The mods and the shader pack that the pack
downloads keep their own licenses.

## Versions

Before 0.1.24 the pack was called Airstrike Pack and the mod Airstrike; older rows use those names.

| version | Incoming | changes |
|---|---|---|
| 0.1.24 | 2.10.3 | The pack is now called Incoming Pack and the mod Incoming (mod version 2.10.3 still shows the old name in game); release files are now `Incoming-Pack.zip` and `Incoming-Pack.mrpack`; installed instances keep updating from the same address. LAN Server Properties removed. Entity Culling no longer stops the client tick of the mod's air defense missile, so its trail does not break. Pack texts in English (README, release notes, optional mod descriptions); the Russian README is now README.ru.md |
| 0.1.23 | 2.10.3 | Mods updated to their latest versions for 1.21.1: Immersive Aircraft 1.6.0 (aircraft need a slightly longer takeoff run; mouse steering is \`, free look is H, see [key bindings](#key-bindings)) and Man of Many Planes 0.2.3 for it, Entity Culling 1.11.3, ModernFix 5.27.26, Flashback NeoForge Fixed 1.0.15; NeoForge 21.1.259, packwiz-installer asks on start whether to update it in the instance |
| 0.1.22 | 2.10.3 | ServerCore brings the chunk tick and simulation distance back up to at most 8 after load. In a test with 6 players, view distance 10 and simulation distance 8 (instead of 12 and 10) cut server thread time by 20 %; Distant Horizons draws the terrain further out |
| 0.1.21 | 2.10.3 | Server performance: Airstrike 2.10.3 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.10.3.md)), strikes no longer load the server tick where nobody sees them; Almighty 0.3.1 ([notes](https://github.com/zentixua/almighty/blob/main/docs/releases/0.3.1.md)), idle game master bots no longer load the server; ServerCore on the server ([settings](#config-files)). In a test with 6 players and a game master campaign across the whole map, server thread time went from 193 to 72 ms per tick (together with the G1 garbage collector on the server) |
| 0.1.20 | 2.10.2 | Mods updated to their latest versions for 1.21.1: Sable 2.0.6 (entities stand on liquids again, like striders), Xaero's Minimap 26.6.0 and World Map 1.47.0, JEI 19.57.0.451 and MezzConfig 0.6.8, Flashback NeoForge Fixed 1.0.14; NeoForge 21.1.256, packwiz-installer asks on start whether to update it in the instance |
| 0.1.19 | 2.10.2 | Airstrike 2.10.2 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.10.2.md)): the glow of distant fires and explosions no longer shows through houses |
| 0.1.18 | 2.10.1 | Almighty 0.3.0 on the server ([notes](https://github.com/zentixua/almighty/blob/main/docs/releases/0.3.0.md)): the game master flies aircraft and steers ships more reliably; when an autopilot rule switches off, the bot releases the controls |
| 0.1.17 | 2.10.1 | Almighty 0.2.2 on the server ([notes](https://github.com/zentixua/almighty/blob/main/docs/releases/0.2.2.md)): building by the game master at the coordinates of a Create Aeronautics ship no longer crashes the server |
| 0.1.16 | 2.10.1 | Airstrike 2.10.1: the server no longer piles up loaded chunks after many salvos; Almighty 0.2.1 on the server: the pilot bot no longer loses its aircraft |
| 0.1.15 | 2.10.0 | spark: TPS, MSPT, memory and profiler for server and client |
| 0.1.14 | 2.10.0 | Chunky removed from the server: together with Distant Horizons it crashed the server on startup (DH calls Chunky before Chunky is loaded) |
| 0.1.13 | 2.10.0 | Key layout without conflicts (Default Options + Balm, see [key bindings](#key-bindings)); in an existing instance, use **Reset Keys** in **Key Binds** |
| 0.1.12 | 2.10.0 | Airstrike 2.10.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.10.0.md)): fixed launcher, salvos from the launch site and along route waypoints, player guide; the game master mod is now Almighty 0.2.0 ([notes](https://github.com/zentixua/almighty/blob/main/docs/releases/0.2.0.md)), server only; Chunky on the server; other mods and settings unchanged |
| 0.1.11 | 2.9.0 | Airstrike 2.9.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.9.0.md)): munitions, reconnaissance, accuracy, aborting your own strikes, nuclear strike without an operator, SAM site; game master mod 0.1.2 on the server; other mods and settings unchanged |
| 0.1.10 | 2.8.1 | Game master mod airstrike-gm 0.1.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.8.2.md)), server only: players download nothing |
| 0.1.9 | 2.8.1 | Airstrike 2.8.1 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.8.1.md)): fixes after a play session; other mods and settings unchanged |
| 0.1.8 | 2.8.0 | MapSyncer: the whole world map from the server in Xaero's World Map; Xaero's World Map and Minimap also on the server |
| 0.1.7 | 2.8.0 | TrueUUID: joining the pack's dedicated server with or without a Minecraft license |
| 0.1.6 | 2.8.0 | Airstrike 2.8.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.8.0.md)); other mods and settings unchanged |
| 0.1.5 | 2.7.0 | More Culling (+ Cloth Config): item frames cost less on the render thread |
| 0.1.4 | 2.7.0 | Distant Horizons is optional (on by default): turn it off on macOS, where DH 3.3.3 crashes the game |
| 0.1.3 | 2.7.0 | Airstrike 2.7.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.7.0.md)); other mods and settings unchanged |
| 0.1.2 | 2.6.0 | Automatic updates (packwiz-installer); LAN Server Properties for joining the host's LAN world without a license |
| 0.1.1 | 2.6.0 | Airstrike 2.6.0 ([notes](https://github.com/zentixua/airstrike/blob/main/docs/releases/2.6.0.md)); other mods and settings unchanged |
| 0.1.0 | 2.5.0 | First version |
