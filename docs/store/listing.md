# Store listing: Incoming Pack

Fields to paste into the Modrinth and CurseForge project forms. The long description is
[description.md](description.md). Checked against `pack/` at version 0.1.24.

## Fields

| field | value |
|---|---|
| Title | Incoming Pack |
| Slug | `incoming-pack` (free on Modrinth: `api.modrinth.com/v2/project/incoming-pack` returned 404 on 2026-10-12) |
| Project type | Modpack |
| Summary | NeoForge 1.21.1 modpack built around the Incoming mod, with Create, Create Aeronautics, aircraft and weapon mods, Distant Horizons and shaders. |
| Minecraft version | 1.21.1 |
| Loader | NeoForge (the pack uses 21.1.259) |
| Modrinth categories | combat, technology, multiplayer (all three are Modrinth modpack categories) |
| CurseForge categories | Tech, Combat / PvP, Multiplayer (names to confirm in the CurseForge form) |
| Environment | Client and server. A server is optional: the pack also runs in single player. |
| License | GPL-3.0-only (the files in the repository; each mod keeps its own license) |
| Source | https://github.com/zentixua/airstrike-pack |
| Issues | https://github.com/zentixua/airstrike-pack/issues |
| Install guide | https://github.com/zentixua/airstrike-pack#installation |

The summary is 143 characters (limit 256) and matches `description` in `pack/pack.toml`, which `packwiz mr export`
writes into the `.mrpack` as its summary.

## Gallery

There are no images in this repository. These images from the Incoming player guide
(`zentixua/airstrike`, `docs/guide/img`) can be used:

| image | size | shows |
|---|---|---|
| https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/nuke.jpg | 1280×480 | nuclear strike over a city, from outside it |
| https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/nuke-wave.jpg | 1280×720 | fireball over a city, with shaders |
| https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/nuke-ruins.jpg | 1280×720 | city after the strike |
| https://raw.githubusercontent.com/zentixua/airstrike/main/docs/guide/img/weapon-shahed.jpg and the other `weapon-*.jpg` | 640×360 | single weapons against the sky |

Screenshots with the HUD (`sam.webp`, `scope.webp`, `hud.webp`, `camera.webp`, `map.gif`, `remote.gif`) were taken
with the game in Russian (for example, `sam.webp` has Russian HUD text) and need English versions before they go into
an English gallery. The `weapon-*.jpg` images are small for a gallery.

Missing:

- An icon. None exists in the three repositories; `zentixua/airstrike` has `tools/trailer/icon_from_frames.py`, which
  makes the mod's icon from trailer frames, but no icon file is committed.
- Screenshots of the pack itself: a Create Aeronautics aircraft in flight, Immersive Aircraft, Big Cannons or Point
  Blank, the view with Distant Horizons and shaders.
- A wide banner image, if the store page should have one.

## Open points

1. **Modrinth: file hosts.** A `.mrpack` may download files only from hosts that Modrinth allows. Modrinth's server
   code (`WHITELISTED_MODPACK_DOMAINS` in `apps/labrinth/.env.local` of `modrinth/code`) lists `cdn.modrinth.com`,
   `github.com` and `raw.githubusercontent.com`; packwiz's export check also allows `gitlab.com`. Modrinth's help page
   on the format (support.modrinth.com) could not be opened from here (HTTP 403). In this pack, 57 mods and the shader
   pack come from `cdn.modrinth.com`; Incoming (`airstrike-<version>.jar`) and Almighty (`almighty-<version>.jar`)
   come from GitHub release downloads on `github.com`, which is allowed. Once Incoming and Almighty have Modrinth
   projects, switching their `.pw.toml` files to Modrinth metadata makes them appear as normal Modrinth dependencies
   of the pack.
2. **CurseForge.** A CurseForge modpack lists its mods by CurseForge project and file ID. This pack has no CurseForge
   metadata at all: 57 mods and the shader pack have Modrinth metadata, and Incoming and Almighty are plain URLs.
   `packwiz curseforge export` puts every file without CurseForge metadata into `overrides/` as a jar, and CurseForge
   reviews such third-party files. A CurseForge version would need each mod added from CurseForge
   (`packwiz curseforge add`), CurseForge projects for Incoming and Almighty, and a check of which mods are not on
   CurseForge at all (not checked here: it needs the CurseForge API).
3. **Almighty in a public pack.** Every server made from the pack installs Almighty. Its bridge is on by default but
   listens only on `127.0.0.1`. Whether the public pack should ship it switched off (`[bridge] enabled = false` in a
   shipped `config/almighty-common.toml`) is for the owner to decide.
4. **Old release file names.** The releases v0.1.22 and v0.1.23 have `Airstrike-Pack.zip` and
   `Airstrike-Pack.mrpack`. Once v0.1.24 is out, `releases/latest/download/Airstrike-Pack.zip` will return 404, so old
   links shared elsewhere need updating. Installed instances keep updating, because the pack address did not change.
