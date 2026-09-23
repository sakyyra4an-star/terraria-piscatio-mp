# Piscatio 0.5.8-mp1 — multiplayer patch

This fork changes only the ItemCheck input dispatch for multiplayer.

## What was wrong

Terraria calls `Player.ItemCheck(int i)` for multiple players in multiplayer.
The original hook deduplicated by tick before checking which player was being
processed. If a remote player was the first ItemCheck in that tick, Piscatio
could consume the tick and never reach the local player's ItemCheck. That
caused the bot to detect bites/statistics while no actual pull/cast happened.

## Patch

The detour now forwards the original `ItemCheck(int i)` argument and processes
automation commands only when `i == Main.myPlayer`. The per-tick guard is then
applied to the local player.

## Build

Target: `i686-pc-windows-msvc`

The original release workflow already builds this target. You can run the
workflow manually after pushing this source to your own GitHub repository,
or build locally with:

cargo build --release --target i686-pc-windows-msvc

Output:
target/i686-pc-windows-msvc/release/piscatio.dll

Replace the old DLL only after completely closing Terraria.
