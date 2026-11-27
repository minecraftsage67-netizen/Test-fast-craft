# FastCraft 0.3

An independent, early-stage Android client for servers speaking **Minecraft PE
0.15.10 (protocol 84)**. Original code and flat colours; no Mojang assets.
Not affiliated with Mojang or Microsoft.

## Status

| Feature | State |
|---|---|
| Ping, RakNet handshake, reliability, split packets, ACK/NACK | Tested |
| Login (offline, no Xbox sign-in) | Tested |
| Chunk loading and meshing | Tested (Rust side) |
| Walking, jumping, step-up, auto-jump, collision | Unit tested |
| Greedy mesher (merges matching block faces) | Unit tested |
| Settings screen, joystick, keyboard, hearts, debug overlay | Written, **never run on a device** |
| Chat, break, place, hotbar select, death/respawn, kick | Tested |
| 3D drawing, touch controls, button editor | Written, **never run on a device** |
| APK build in GitHub Actions | Written, **never run** |

"Tested" means against a small test server built from PocketMine-MP's real
0.15.10 packet classes and RakLib session code. It is **not** a full
PocketMine server, and no real server or real game client has been tried.
Other server software may behave differently.

## Not implemented

- Inventory screen, crafting, chests, furnaces, signs
- Item names and icons (the hotbar shows block ID numbers)
- Combat, eating, bows, dropping items, armour
- Creative flying, boats, minecarts, sleeping
- Image textures, mob models, name tags, animations, lighting, day/night, sound
- A renderer written in Rust (drawing is still Java OpenGL ES 2)
- Mouse look
- Dimensions (Nether), redstone visuals, tool speed bonuses when mining
- Xbox Live sign-in, server list, world saving, single player
- Plugin windows and forms beyond chat
- Creative mode offers only the first nine blocks of the creative list

## What is new in 0.3

- Greedy meshing: about 59% fewer quads on generated hilly terrain
  (18,891 to 7,709). Building a chunk took a little longer on a PC
  (2.0 ms to 2.4 ms). Frame rate on a phone has not been measured.
- Frustum culling, near-to-far drawing and an optional FPS limit.
- Settings: field of view, sensitivity, invert look, auto-jump, FPS limit,
  fog, block pixel pattern, view bobbing, sprint view, hotbar size,
  crosshair and debug info.
- Joystick button and preset (drag past the top to sprint).
- Hardware keyboard: WASD, Space, Left Shift (sneak while held; a Sneak
  button toggles), Left Ctrl, 1-9, T for chat, F3 for debug info. Mouse
  wheel changes slot.

## Build the APK (GitHub Actions)

1. Put the contents of this folder at the root of a GitHub repository,
   including the hidden `.github` folder.
2. Open **Actions → Build APK → Run workflow**.
3. Download the **FastCraft-apk** artifact and install `app-debug.apk`.

Supports Android 7.0+ on arm64-v8a and armeabi-v7a.

## Test on a PC

    cargo test
    cargo run -- 127.0.0.1:19132          # ping and handshake

## Layout

    src/lib.rs        ping and opening handshake
    src/raknet.rs     connection reliability
    src/protocol.rs   0.15.10 packets
    src/world.rs      chunks, physics, ray casting, meshes
    src/game.rs       game loop and the pipe protocol to the app
    src/codec.rs      bytes, base64, zlib
    android/          the app (menu, game screen, renderer, button editor)

The app runs the Rust core as a child process: commands go in as text lines,
and world data comes back as binary frames.

## Controls

Drag to look, tap to place or use, hold to break the block under the
crosshair. In **Custom buttons**, drag a button to move it and tap it to
resize, fade, rename or delete it. A "Chat text or /command" button sends
your own text, such as `/spawn`.
