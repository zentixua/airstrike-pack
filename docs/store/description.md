A NeoForge modpack for Minecraft 1.21.1 built around the [Incoming](https://github.com/zentixua/airstrike) mod. Before
version 0.1.24 the pack was called Airstrike Pack and the mod Airstrike.

![A nuclear strike seen from outside the city](https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/nuke.jpg)

## Incoming

You aim strikes with a strike designator and its binoculars, or from a targeting map for distant targets. The
weapons are a kamikaze drone, a loitering munition, a cruise missile, a rocket artillery salvo, a B-2 bomber with a
bunker buster bomb and an ICBM with a nuclear warhead. A target can be a block, a mob, a player or a Create
Aeronautics aircraft, including one in flight. Projectiles are built at real size, with one block as one meter, and
can be watched from an onboard camera. A SAM site shoots down drones, loitering munitions, cruise missiles and the
B-2. Munitions are crafted, and a fixed launcher fires on a redstone signal.

The [player guide](https://github.com/zentixua/airstrike/tree/main/docs/guide) covers every weapon, the controls and
the commands.

![The fireball of a nuclear strike over a city](https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/nuke-wave.jpg)

## The rest of the pack

- Create and Create Aeronautics for building machines, aircraft and ships, with Create Propulsion: Simulated,
  Aeroworks, Tweaked Controllers, Big Cannons, Connected, Deco and Diesel Generators.
- Immersive Aircraft with Man of Many Planes, and Vic's Point Blank firearms.
- Distant Horizons for terrain far beyond the view distance (optional, on by default; turn it off on macOS, where
  DH 3.3.3 crashes the game).
- Iris with the Complementary Reimagined shader pack and Euphoria Patches.
- Xaero's Minimap and World Map. On a server, MapSyncer sends players the whole world map.
- Flashback for recording replays (optional, off by default).
- A key layout in which no two mods share a key where both could fire (Default Options).
- Sodium, Lithium, FerriteCore, ModernFix, ImmediatelyFast, Entity Culling, More Culling and Dynamic FPS for
  performance.

The full list, the shipped config files and the reason for each are in the
[README](https://github.com/zentixua/airstrike-pack#whats-inside).

## Installing

Install the pack from this page in Prism Launcher or the Modrinth App. When the launcher asks about optional mods,
tick Distant Horizons unless you are on macOS; the recording mods are only needed for replays.

For a pack that updates itself before every game start, import `Incoming-Pack.zip` from the
[latest GitHub release](https://github.com/zentixua/airstrike-pack/releases/latest) in Prism Launcher. Details are in
the [README](https://github.com/zentixua/airstrike-pack#installation).

## Servers

A server installs the same pack with packwiz-installer (`-s server`). Two mods are installed only on the server:
ServerCore for performance and [Almighty](https://github.com/zentixua/almighty), a bridge through which an AI agent
can run the game as game master. Almighty's bridge listens on `127.0.0.1` by default and needs a token for every
call. TrueUUID lets a server with `online-mode=false` check players' accounts itself. Setup is in the
[README](https://github.com/zentixua/airstrike-pack#running-a-server).

## License

The pack's own files are licensed under GPL-3.0-only. Each mod keeps its own license.
