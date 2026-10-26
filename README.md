# Piconet

Piconet is online multiplayer for F-Zero GX on PC: races of up to 30 machines, players and bots on equal seats,
with rollback so that every player's game stays in step. The game runs as native code, recompiled from the
GameCube's; the launcher prepares it from your own disc, finds races, starts the game, keeps replays and offers solo
practice.

Website: [piconet.live](https://piconet.live). Discord: [discord.gg/DcauFmsGh](https://discord.gg/DcauFmsGh).

To play, download the launcher from [piconet.live](https://piconet.live) or from the
[launcher's releases](https://github.com/piconet-live/piconet-launcher/releases) on GitHub.

<!-- Screenshot: the launcher, added later. -->

No game data is in any of these repositories. You need your own copy of F-Zero GX (USA) for the GameCube.

## The parts

| Repository | What it is |
|---|---|
| [piconet-launcher](https://github.com/piconet-live/piconet-launcher) | The program a player opens: online races, practice, replays, the game's preparation from your disc |
| [piconet-runner](https://github.com/piconet-live/piconet-runner) | F-Zero GX recompiled to native code, and the port that makes it race online |
| [piconet-protocol](https://github.com/piconet-live/piconet-protocol) | The formats and messages the parts share: the relay's network packets, the launch, replay and match files, and the status board between the launcher and the game; each described once, the C# and C++ code generated from it. A submodule of the others |
| [piconet-tools](https://github.com/piconet-live/piconet-tools) | `Fzgx.GameCube`: readers for the disc and the game's file formats |
| [dolrecomp](https://github.com/piconet-live/dolrecomp) | Our fork of DolRecomp, the recompiler (branch `piconet`) |
| [recompcore](https://github.com/piconet-live/recompcore) | Our fork of RecompCore, the emulator core the runner is built on (branch `piconet`) |
| [moderngekko](https://github.com/piconet-live/moderngekko) | Our fork of ModernGekko, the runtime (branch `piconet`) |

The online service that runs the lobbies and the relays is not public.

## Building

Each repository's README lists its requirements, each with its install command, and how to build and check it.
Clone with git inside WSL (or on Linux), not Git for Windows, whose `core.autocrlf` would break the `.sh` files.

The runner and the launcher are built side by side: clone piconet-runner and piconet-launcher into the same folder.
The runner's Windows build needs its work folder on a Windows drive (no spaces in its path). The clone can be in
WSL's own file system, with the work folder on a Windows drive (preferred), or on a Windows drive itself; under
`/mnt/<drive>` git is much slower (a `--recurse-submodules` clone took about 24 minutes there, against 6). The
runner's clone needs its own submodules but not theirs (its setup fetches what the build needs):

    git clone https://github.com/piconet-live/piconet-runner.git
    git -C piconet-runner submodule update --init
    git clone https://github.com/piconet-live/piconet-launcher.git
    git -C piconet-launcher submodule update --init

The runner's clone is about 1.1 GB and its work folder about 5 GB; plan for about 25 GB in all. A first build takes
about 40 minutes (the module about 12, the runner about 10). The steps are in
[piconet-runner's README](https://github.com/piconet-live/piconet-runner#building).

## Contributing

1. Fork the repository you want to change.
2. Make a branch for your change.
3. Open a pull request against `main`. Its README's checks must pass; CI runs them.

A change to a file format or a message goes to piconet-protocol first; the parts then take it with a submodule bump.
Please discuss bigger changes on the [Discord](https://discord.gg/DcauFmsGh) before you start on them. Bugs and ideas
are welcome as issues, or on the Discord. Each repository's `CONTRIBUTING.md` has the details.

## Credits

- The Piconet logo by Emarcee.
- [DolRecomp](https://github.com/ExpansionPak/DolRecomp), [RecompCore](https://github.com/ExpansionPak/RecompCore)
  and [ModernGekko](https://github.com/ExpansionPak/ModernGekko) by ExpansionPak: the recompiler, the emulator core
  and the runtime Piconet's game is built on.
- [Dolphin](https://dolphin-emu.org), the GameCube and Wii emulator RecompCore is built from.
- The stat pack authors, whose packs Piconet ships with their permission: Emarcee (F-Zero GX+), and AKC12 and
  Twilight (GX Unleashed).

## Licence

The launcher, the runner, the protocol and the tools are GPL-3.0-or-later. The forks keep their upstreams'
licences.
