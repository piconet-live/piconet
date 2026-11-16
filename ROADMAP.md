# Roadmap

What Piconet is working toward: the next release in detail, the direction after it, the rules every change follows,
and the ideas that are parked. This file changes as work lands and plans change.

Last updated: 16 November 2026.

Please talk about an item on the [Discord](https://discord.gg/DcauFmsGh) before you start on it.

## How to read this file

It is written for two readers: people who want to contribute, and programs (LLM agents) that need to know what is
planned, what is decided, what binds each item and where it stands. It is long on purpose.

- **IDs.** Every item has a stable ID. IDs are never reused; an item that is dropped keeps its ID and says so.
  - `D1`..: direction.
  - `P1`..: principles.
  - `M0`..`M8`: the 0.7 milestones; `M4.6` is item 6 of milestone M4.
  - `T1`..: 0.7 work that spans milestones; `T2.3` is item 3 of T2.
  - `L-XXX`: long-term items (`L-TA` is Time Attack).
  - `I-XXX`: ideas that are not planned.
- **Status words.**
  - **Done**: built and checked in the current development build.
  - **In progress**: being built now.
  - **Partly**: some of it exists; the note says what.
  - **Not started**: planned, nothing built.
  - **Decided**: the design is settled, nothing built.
  - **Proposal**: suggested, not decided.
  - **Parked**, **On hold**, **Shelved**, **Paused**, **Dropped**: in the ideas section, with the reason where known.
- **Dates** are the day a decision was taken. Where two decisions disagree, the later one stands; earlier ones are
  only mentioned where they would otherwise be mistaken for current.
- **The native program is in development and not public yet.** Its status in section 4 is described from the
  development build. Everything a contributor can work on today is in the public repositories listed in section 1.

## Contents

1. [Where things stand](#1-where-things-stand)
2. [Direction](#2-direction)
3. [Principles](#3-principles)
4. [Short term: 0.7, the native game](#4-short-term-07-the-native-game)
5. [Long term](#5-long-term)
6. [Ideas, not planned](#6-ideas-not-planned)
7. [How this file is kept](#7-how-this-file-is-kept)

## 1. Where things stand

**Released: 0.6.5 (13 November 2026).** The launcher, the recompiled game (the runner), the protocol and the online
service. Races of up to 30 machines, players and CPUs on equal seats, rollback netcode, Quick Play with a course and
mode vote, custom lobbies with host mode files, LAN, practice with rewind, replays recorded by default and played
back with a scrubber, spectating, rejoining a race, stat packs (GX Unleashed, F-Zero GX+), mods, high refresh by
run-ahead (real frames up to 240 a second), ray tracing labelled Experimental.

Until 0.7 ships, 0.6.5 is the current version.

**In development, not public yet: 0.7, the native program.** The game is built from the complete, matched
[F-Zero GX decompilation](https://github.com/karamzov123/fzero-gx-decomp) instead of being recompiled from the
GameCube's machine code, and the launcher's work moves into it. State on 16 November 2026:

- The race step runs natively and equals the recompiled game's step for step. First proof 13 November: a 30-machine
  race, 0 differing steps. The whole native step takes about 0.22 ms; the machines' update and the 30 CPU drivers
  run 8 to 10.6 times faster than the same code in the recompiled game.
- The menus run over a live demo race: title screen, main menu, Garage, Options, Course & Music, Solo races against
  29 CPUs with a pause menu and rewind, the Quick Play lobby, chat, and an online race over the relay.
- Two native programs (one Linux, one Windows) raced each other with eight test players over a local server with
  60 ms of added lag: 6953 steps, 0 checksums and 0 states differing, the same results on both, and both replays
  play back equal in the recompiled game.

**Where to help today.** Contributions go to the public repositories:

| Repository | What it is |
|---|---|
| [piconet-runner](https://github.com/piconet-live/piconet-runner) | The recompiled game, as released in 0.6.5 |
| [piconet-launcher](https://github.com/piconet-live/piconet-launcher) | The launcher |
| [piconet-protocol](https://github.com/piconet-live/piconet-protocol) | Messages, the mode language and its compiler, stat packs |
| [piconet-tools](https://github.com/piconet-live/piconet-tools) | Readers for the disc and the game's file formats |
| [dolrecomp](https://github.com/piconet-live/dolrecomp), [recompcore](https://github.com/piconet-live/recompcore), [moderngekko](https://github.com/piconet-live/moderngekko) | Our forks of the recompiler, the emulator core and the runtime the recompiled game is built with |

The decompilation is complete (L-DECOMP).

## 2. Direction

| ID | Direction | Decided |
|---|---|---|
| D1 | **One program, one game.** Piconet becomes one program that is the whole game: menus, lobbies, Garage, replays, chat and races in one window, drawn by one renderer. The launcher and the separate game window go. | 2026-11-13 |
| D2 | **The bar is a new F-Zero.** The whole thing should plausibly pass as an F-Zero GX 2, not a launcher around an old game. A new art style is welcome if it gets there. | 2026-11-13 |
| D3 | **No loading screens.** Races start at once, as in Trackmania: every place in the program (menu, Garage, lobby, race, replay, practice) is its own object in memory, and moving between them shows no load and no cut. | 2026-11-13, widened 2026-11-15 |
| D4 | **The racing stays exactly the game's.** What changes is everything around the race. Any departure from the original is an explicit switch (P3). | standing |
| D5 | **The loop.** After 0.7: Time Attack, a daily cup, Create and ranked seasons (section 5.1). | 2026-11-13 |
| D6 | **The name is Piconet.** The program is `Piconet.exe`. The "Piconet Launcher" name retires with the launcher. | 2026-11-15 |
| D7 | **Windows and Linux in 0.7** (T4); ARM later (L-ARM). | 2026-11-14 |

## 3. Principles

Every contributor follows these. Each is a rule and the reason for it.

| ID | Rule | Why |
|---|---|---|
| P1 | **Console-faithful exactness.** Everything a race computes is bit-exact to the game. Nothing a race reads changes unless a rule (P3) says so. The long-term aim is that a Piconet replay of an offline race could be converted into a replay the stock game's own viewer plays (the game's format holds at most four pad-driven machines, so an online race of 30 cannot be converted whole). | Rollback, replays, time verification and every speed gain stand on it. |
| P2 | **Native is proven by equal results.** Every check of the native program is differential: native against the recompiled game on the same inputs, compared per step (simulation state and checksum), per frame (pictures) and per sample (sound). Inputs are chosen by the code they reach (function and branch coverage), not by how many replays, modes or builds they cover. A difference found is fixed with its cause stated. The matched decompilation already compiles byte-identical to the retail game, so checks aim at our layer: generated code, hand-written replacements for assembly-only routines, our patches rewritten in C, compiler and platform behaviour (float rules, undefined behaviour, layout). | The recompiled game is the verified reference; testing game behaviour again proves nothing extra. |
| P3 | **Deviations are switches.** Anything that departs from what the developers intended (a damage cap, immunity, wreck behaviour, a HUD change) is its own switch or mode word, with the game's own behaviour as the default, bit-identical when off. A deviation is never bundled into another rule. | The original game is the reference; changes must be visible and chosen. |
| P4 | **A pause never changes the race.** A paused race must come out exactly like the same race never paused. Anything the simulation reads that changes during a pause (a random draw, a counter) is held fixed. Proof for any pause work: the paused race equals the straight race step for step. | Pausing is not gameplay. A pause that shifts the race breaks rewind, practice and replays. |
| P5 | **60 fps with 30 players, always.** No dropped frames in races, including the worst rollback frame. No added input delay (the native program has none). A correction of your own machine or camera is shown as it is, never eased. | A full race must be smooth for everyone; what you see of your own machine must be the state your inputs act on. |
| P6 | **The UI never blocks.** Device, disc, file, network and decode work stays off the frame's thread. A page opens at once and fills in when its data arrives. | Every stall is felt. |
| P7 | **Our code stays out of the game's memory.** Our art, textures, buffers and state live in host memory, never in the game's heaps. Reading game data is fine; what we keep is copied out. | The game's heaps are sized for its own needs; anything of ours there starves the game and fails in far-away code. |
| P8 | **A limit is a heap or a layout.** When something does not fit, first name which. A heap's fixed size: raise it, or give it to the object that owns it. A layout (fixed arrays, hard counts such as the 30-racer cap, 32-bit offsets): that piece moves to free native structs (L-STRUCT). | The fix depends on the kind of limit. |
| P9 | **One camera rule.** Whatever view is shown (menu, title flight, course switch, lobby, Garage, spectating, replays), the game's own camera is set to it, so the game's culling, level of detail, effects, 3D sound and fog follow it. No feature overrides only the drawn view. After any rollback or restore, the camera is applied again. | A view-only override draws a wrong world. |
| P10 | **No desync repair.** A race that desyncs stays desynced; checksums detect and report it. No game ever sends state to another. | Simplicity, and no player may push state onto others. Improve determinism instead. |
| P11 | **Older replays keep playing.** Replays from every released build list, play and pass the replay check in every later build. A change to what a race computes is carried as a rule the replay records, so old replays keep the old behaviour. | Players keep their replays. |
| P12 | **One rollback library.** Online rollback, practice rewind, the replay scrubber, rejoining and spectating all use the one rollback implementation over the undo log. Any new save or restore need goes through it. | A second rollback is a second exactness risk. |
| P13 | **The UI explains itself.** No helper sentences, captions or hints on screen: state shows by form, colour and motion. No taglines in player copy. Settings use the names PC players know (Resolution, Anti-aliasing, Vertical sync). A setting has one name everywhere. An option lives on the thing it adjusts, not as a global knob. | A game's screens are read at a glance. |
| P14 | **One way to build.** One repository per part, one build system with a preset per platform, CI building each platform, a README listing requirements. The Linux build stays bit-exact with the Windows build. | An outsider can only follow one documented path. |
| P15 | **Reuse, never reinvent.** Use existing libraries, tools and conventions. This applies to our own code first: native work starts from the recompiled game's existing fixes and systems, ported as they are, never solved again; native high refresh is the core's own run-ahead, not a copy. | Custom tooling is code to maintain; a second version of a working fix loses what the first learned. |
| P16 | **Everyone runs the same engine.** No races between different engines, no fallback interpreter for players. The CPU floor is x86-64-v3 (AVX2; PCs newer than 2019). Choices that would block an ARM port with identical results are avoided. | Rollback and checksums need identical results on every PC. |
| P17 | **Fixes are general.** No lists of course, model or texture ids and no per-course values: a fix is a rule that works on every course, custom ones included, found at its source in the game. Tags identify the game's objects; they never restyle them. | Custom tracks and machines must work with no per-asset data from us. |
| P18 | **Unreleased work has no compatibility path, and removals sweep.** Something that never shipped in a released build is replaced outright. Dropping a feature removes every setting and word it brought in the same change. | Shims for unshipped words are dead code; leftover settings confuse players. |
| P19 | **Versus mode is the source of truth for race events.** The order of finish, KO, race end and results follows the game's versus mode; departures only where 30 seats or the network force them. | Versus already handles several humans correctly. |
| P20 | **GameCube controllers go raw to the game.** A GameCube adapter port passes buttons and sticks to the game with no remap or calibration of ours; the game's own controller settings apply. Other pads and keyboards get our mapping. | GameCube players expect the console's feel. |

## 4. Short term: 0.7, the native game

### 4.1 What 0.7 is, and the decisions that bind all of it

0.7 replaces both the recompiled game and the launcher with one native program, built from the decompilation with
[Aurora](https://github.com/encounter/aurora) for graphics and the window. It ships for Windows and Linux. It is in
development and not public yet.

| ID | Decision | Date |
|---|---|---|
| 0.7-a | The native program replaces the launcher at release. No period with both supported. | 2026-11-14 |
| 0.7-b | The launcher's player-facing C# logic is ported to C++ inside the native program. The lobby messages are generated from the protocol's message definitions for both languages, so the two cannot drift. No .NET runtime in the program. C# stays for the server, tests and offline tools. This replaces the earlier plans of calling the launcher's C# as a library and of an HTML/CSS UI library. | 2026-11-14 |
| 0.7-c | Release scope: everything the launcher does today has a home in the native program, plus patch notes in the game, plus Linux. Accounts and crash upload are not in the release scope unless added. | 2026-11-14 |
| 0.7-d | Grand Prix, Story and Time Attack with ghosts are in the first native build. | 2026-11-13 |
| 0.7-e | Race state stays in the game's own memory layout. Free native structs are a separate, later project (L-STRUCT). | 2026-11-12 |
| 0.7-f | Checking native is P2: equality with the recompiled game on chosen inputs, not a sweep of every replay and mode. | 2026-11-13 |
| 0.7-g | The recompiled game and the launcher keep serving players until 0.7 ships. Every version retires the one before it; clients update themselves. | 2026-11-14 |
| 0.7-h | One current development build; finished work lands in it at once. | 2026-11-15 |
| 0.7-i | Every place in the program is its own object (T5). | 2026-11-15 |

### 4.2 Milestones at a glance

| ID | Milestone | Player-visible result | Status |
|---|---|---|---|
| M0 | Races from the menu | Solo: choose a course and a machine, race 29 CPUs, see the results, back to the menu, in one process, no loading screen | Done |
| M1 | Player data and Options | Options in the game; a launcher player's settings and controller mapping carry over | Mostly done |
| M2 | First start, alone | Fresh install, pick your disc, playing, with no launcher | In progress |
| M3 | Practice, replays, stats | Practice with rewind from the menu; races saved; replays play and scrub in the game; a double-clicked replay opens | Partly |
| M4 | Online I: Quick Play | Quick Play, lobbies, the vote, chat, 30-player races, watching, rejoining | In progress |
| M5 | Online II: custom lobbies, LAN, packs | Custom lobbies and LAN; mode files and stat packs; custom machines online | Started |
| M6 | Mods | Import, order and switch mods; mods offline, race-safe parts online | Not started |
| M7 | Platform | Updates, Discord presence, lobby alerts, patch notes in the game | Not started |
| M8 | Release | Installers for Windows and Linux; the servers accept 0.7; launcher installs move over; launcher and recompiled game retired | Not started |

Work that spans milestones is in section 4.4 (T1 to T7).

### 4.3 The milestones

#### M0: Races from the menu

The demo race behind the menu hands over, a race starts, it ends at its results, and the demo and the menu come back,
as often as asked, in one process.

| ID | Item | Status | Notes |
|---|---|---|---|
| M0.1 | Solo, RACE page (course, machine), race 29 CPUs, results, back to the menu | Done | Races started this way equal, over all 2838 steps, the same race started fresh. |
| M0.2 | After a race: RETRY, NEXT COURSE, MAIN MENU | Done | Each choice starts a race equal to its fresh twin. |
| M0.3 | Laps and CPU level on the RACE page | Done | CPU level Novice to Master. Unlimited laps left out: a free run never reaches results. |
| M0.4 | A race starts as a fresh boot's would | Done | A race started after demo races first carried left-over state from them, which desynced an online race. Fixed 2026-11-15. |
| M0.5 | Freeze detection in place of the launcher's | Partly | A watchdog for the window exists. A race freeze watch equal to the launcher's is not confirmed. |

- **Decided:** in Solo the player starts on the game's own grid (last); the online grid rule is the server's.

#### M1: Player data and Options

| ID | Item | Status | Notes |
|---|---|---|---|
| M1.1 | Settings file in the launcher's format | Done | The launcher's settings file is read and written, all 49 members. Unknown members are kept; a broken file is never written over. |
| M1.2 | Options pages | Done | DISPLAY, GRAPHICS, SOUND, CONTROLLER, ONLINE, REPLAYS, GAME. Picture settings apply while running, ray tracing, anti-aliasing and resolution included. |
| M1.3 | Controllers | Done | Four ports; each of the 20 controls set by capture; stick dead zones; rumble test; the game's own controller menu from Options. A GameCube adapter port passes raw (P20). A run with a real adapter is still to be checked. |
| M1.4 | Carry over a launcher player's data | Partly | The program can read a 0.6.5 data folder as it is. The first-start step that finds and adopts it is M8.3. |
| M1.5 | New settings of the native program | Done | Motion blur and its strength, music and effects volume, Show inputs (in the picture or in a separate window), Follow the game's progression (T3), Show other players' emblems (on by default). |
| M1.6 | Input delay | Done (removed) | Locked to 0 in every race (2026-11-15). |
| M1.7 | Settings that wait on their feature | Not started | Exclusive full screen, server choice, notices, chat, automatic updates (M7.1), Discord (M7.2), Garage sorting (T2). |

- **Open:** the launcher's controller-navigation setting; whether and how it applies to the native menu is not
  decided. On Linux, typed characters do not yet reach text fields.

#### M2: First start, alone

| ID | Item | Status | Notes |
|---|---|---|---|
| M2.1 | Disc check | Done | The launcher's check, ported and held equal to it. |
| M2.2 | Accept a 0.6.5 prepared game folder | Done | |
| M2.3 | How the disc is used: copy its files out once, or check the image once and read it in place | Open | Reading in place could also accept compressed image formats. |
| M2.4 | Disc picker and progress page | Not started | A file dialog; the check off the frame's thread with progress and cancel (P6). |
| M2.5 | Graphics prepared while the menu runs | Done | Shaders are compiled in the background while the menu runs. A cold start takes about 4 minutes of menu time; later starts need nothing. |
| M2.6 | CPU check with a message | Not started | The program needs x86-64-v3 (P16); an older CPU must get a plain message, not a silent crash. |
| M2.7 | Where the program keeps the disc | Not started | A first-start setting. |

#### M3: Practice, replays, stats

| ID | Item | Status | Notes |
|---|---|---|---|
| M3.1 | Rewind in Solo races | Done | START opens a pause menu (CONTINUE, RETRY, REWIND, QUIT) with a rewind timeline; save and load on the D-pad. |
| M3.2 | The pause as a held step (P4) | Done | A paused race equals the same race never paused, step for step; the music plays on, race sounds stop and come back. |
| M3.3 | Practice page | Not started | The Solo menu's PRACTICE row does nothing yet. What it holds beyond Solo races with rewind is open. |
| M3.4 | Recording | Done | Online races record by default; Solo races record with Record practice on. |
| M3.5 | Replays page | Not started | The REPLAYS row does nothing yet. Playback itself works: a replay plays back step-equal. |
| M3.6 | The replay scrubber's bar | Not started | The scrubbing works in the core; its bar is not drawn natively. |
| M3.7 | A double-clicked `.fzrp` opens in the program | Not started | The launcher does this. |
| M3.8 | Stats page | Not started | A real stats system waits for accounts (L-ACC). |

- **Binding:** every replay format the launcher reads (up to version 8: xz-compressed, column inputs, a one-byte
  checksum every step) stays readable (P11).

#### M4: Online I: Quick Play

| ID | Item | Status | Notes |
|---|---|---|---|
| M4.1 | The lobby client in C++ | Done | Ported from the launcher's C#, held to it by vectors the C# records: 6310 checks and 11 session scenarios (186 steps), 0 differences. |
| M4.2 | WebSocket and TLS | Done | curl 8.23.0; Windows uses the system's TLS, Linux mbedTLS 3.6.7 LTS. A network thread owns the link; the frame thread never waits on it. Keep-alive, reconnect with backoff. |
| M4.3 | Relay measurement | Done | All relays pinged at once. |
| M4.4 | Online pages: ONLINE, QUICK PLAY, the lobby | Done | ANY LOBBY or one of the open lobbies, joinable at any stage (a racing lobby takes you for its next race). The lobby: roster, READY, Garage, Leave (held). Custom lobbies and LAN show as coming. |
| M4.5 | Vote, CPU vote, machine choice | Done | The course vote as a hand of cards (rows of at most five, Random its own row, a card flips to show its mode and rules), the draw animated; the machine choice as a hand of machines. |
| M4.6 | The race over the relay | Done | The core's relay session, reports to the server, the end of the race and the way back to the lobby. Proof in section 1. |
| M4.7 | Why a race did not start or was left | Partly | The reasons are handled but not shown. |
| M4.8 | Chat and Social | Done | Social is a panel in the corner: lobby players as emblem cards, lobby and global chat, a dot for new lines (never a count). Hide swear words held equal to the launcher on 7668 texts. Friends and Recent wait for accounts. |
| M4.9 | Server status and notices | Partly | The status (players, lobbies) is shown. Showing notices is not confirmed. |
| M4.10 | Emblems | Done | Every racer's emblem drawn on their machine by the game's own emblem code; other players' emblems on by default, with a switch. |
| M4.11 | The lobby's camera | Done | The lobby's course shown through one of the course's own TV camera shots, held still; the demo moves to the voted course. |
| M4.12 | Watching a live race | Not started | The core can spectate; the native program does not yet. |
| M4.13 | Rejoining | Not started | The core can rejoin; the native program does not yet. |
| M4.14 | Rollback speed | Partly | A 10-step rollback frame takes 3.8 ms against the recompiled game's 9.5; a 21-step rollback on 30 seats 5.5 ms median, 9 ms at most, on the development PC. The rollback worker (deep rollbacks on a second core) and per-machine rollback are needed natively and not native yet. |
| M4.15 | The rest of the lobby flow | Decided, not started | Approved as a direction on 2026-11-15: the vote opens on the final lap; while waiting, spectate, warm up or visit the Garage; a flight and a rolling start (T7) into the race; results with you first; a rival mark; quick chat. |

- **Decided (2026-11-16):** the native program carries both the rollback worker (deep rollbacks on a second core) and
  per-machine rollback, as the recompiled game does.
- **Decided:** no global list of online players until accounts exist and players can hide their status.
- **Depends on:** M0, M8.2.

#### M5: Online II: custom lobbies, LAN, packs

| ID | Item | Status | Notes |
|---|---|---|---|
| M5.1 | The `.fzmode` compiler in C++ | Done | A port of the protocol's C# compiler. Gate: 2480 sources, 5009 packages and 1111 modes over 4951 parameter sets give the same packages, hashes, errors and race terms as the C#; clean under ASan and UBSan. |
| M5.2 | Offline races with your own mode file | Partly | The program races a given mode file; choosing one in the menu is not built. |
| M5.3 | Custom lobbies | Not started | Browser, create, join, password, the host's settings, start. |
| M5.4 | LAN | Not started | |
| M5.5 | Stat packs | Partly | The race applies packs. Not yet: checking a match's pack, a host's own pack, packs per seat. |
| M5.6 | Custom machines online | Partly | The lobby sends the Garage's build; not yet checked end to end in a native online race. |

- **Decided (mode system):** a lobby's own settings are only its name, password, player limit, ping limit, timers,
  voting minimum and how the course is picked. How the race plays comes from the mode file and its parameters
  (L-MODE).

#### M6: Mods

| ID | Item | Status | Notes |
|---|---|---|---|
| M6.1 | Import, order, switch on and off | Not started | Mods as file overlays, as in the launcher. |
| M6.2 | Mods online | Not started | Only parts that cannot change the race are used online. |
| M6.3 | Graphics prepared again after a change | Not started | |
| M6.4 | The Garage's MODS shelf | Partly | The shelf exists (T2.2); M6 fills it. |

- **Decided:** machine models from mods move later to a skins area in the Garage.

#### M7: Platform

| ID | Item | Status | Notes |
|---|---|---|---|
| M7.1 | Self-update | Not started | Never during a race. Which update library is open. |
| M7.2 | Discord Rich Presence | Not started | |
| M7.3 | Alerts at lobby moments | Not started | |
| M7.4 | Patch notes in the game | Not started | In release scope (0.7-c). The same notes as the GitHub release. |
| M7.5 | Report a problem | Not started | Not in release scope unless added (0.7-c). |

#### M8: Release

| ID | Item | Status | Notes |
|---|---|---|---|
| M8.1 | Packaging | Not started | An installer for Windows and a package for Linux. |
| M8.2 | The servers accept 0.7 | Not started | Older lines refused with an update message. Open: the native build's identity, since a Linux and a Windows program of one version must share lobbies. |
| M8.3 | Migration from 0.6.x | Not started | The last launcher update leads to the native program, which adopts the launcher's data (settings, controller files, replays, prepared game files). |
| M8.4 | The launcher and the recompiled game retired | Not started | |

### 4.4 Work that spans the milestones

#### T1: The menu over the always-running demo race

The menus are views over one demo race that never ends, the camera on the player's own machine (driven by a CPU).
Every place in the menu is a camera and what is drawn over that race, not a separate scene. Decided 2026-11-13,
refined since.

| ID | Item | Status | Notes |
|---|---|---|---|
| T1.1 | Title screen | Done | Everything before START is GX's own title: its logo (read from the player's disc), PRESS START, the attract camera, the title music. START flies the camera to the player's machine and the main menu opens. |
| T1.2 | Main menu | Done | SOLO, ONLINE, GARAGE, REPLAYS, OPTIONS, EXIT, in Direction D (the project's art style) built in 3D. |
| T1.3 | Camera rules | Done | Hovering never moves the camera; choosing moves it to the new page. Text is legible from a dark backing behind the letters; the race behind is not blurred. |
| T1.4 | Drive the demo | Done | X on the main menu drives the demo race; only the speed of the HUD shows; START hands it back. Race sounds are muted under the menus. |
| T1.5 | Seamless course switch | Done | The next course's race loads and runs beside the current one; the camera circles and fades into it. |
| T1.6 | Course & Music | Done | COURSE: every course as a card with its track in 3D. MUSIC: every track on the disc, playlists, favourites, each course's music chosen by the player, a theme's final-lap part on the final lap. A course's default music is the game's own. |
| T1.7 | Each menu item a designed piece | In progress | Each option to be its own graphic composition, not a word on a plate. Ongoing design. |

#### T2: The Garage

The Garage stands over the running race; the machine is edited there and swapped into the race on leaving.

| ID | Item | Status | Notes |
|---|---|---|---|
| T2.1 | Stations round the machine | In progress | CHOOSE, PARTS, PAINT, EMBLEM (dimmed until accounts), SETTING, and SHOP when the game's progression is followed. The C-stick swings between them. |
| T2.2 | CHOOSE | Done | Shelves CUSTOM, STOCK, MODS, opening on STOCK; the machines as 3D models. A player who never chose starts on Wild Goose. |
| T2.3 | PARTS | Done | A 5 x 5 grid per kind (body, cockpit, booster). |
| T2.4 | PAINT and SETTING | Done | SETTING is the game's own speed and acceleration setting. |
| T2.5 | Live swap and music | Done | The racing machine becomes the build on leaving; the Garage has its own music. |
| T2.6 | Parity with the game's garage | Partly | Done: all machines and pilots, own paint, the setting, saved builds load and list. Not done: deleting and reordering builds, typing a hex colour, the trail colour, the emblem editor (with accounts), mouse use (untested). |
| T2.7 | SHOP | Done | Only with Follow the game's progression on, with the game's own prices and rules. |

- **Proposal, not decided:** the Garage's overall layout. The station structure is being tried, and parts of it
  have changed with each try; it is not a settled design. The launcher-era Garage proposals were never approved as a
  whole.
- **Decided:** the Garage supports everything the game's garage had; emblems are stored with accounts.

#### T3: Single player is the base game

| ID | Item | Status | Notes |
|---|---|---|---|
| T3.1 | Grand Prix, Story and Time Attack with ghosts, as the game shipped them, at high refresh | Not started | In the first native build (0.7-d). The Solo menu lists them; they do not start yet. Story courses already race natively in the equality checks. |
| T3.2 | Everything unlocked, or the game's progression | Done | Everything is unlocked by default, and online always has everything. Options > Game > Follow the game's progression locks machines and parts by the game's own save and adds the SHOP. |
| T3.3 | Import a GameCube save (`.gci`) | Partly | Works; no file picker yet. |
| T3.4 | Tickets | Not started | Earned only in Grand Prix and Story, so they wait for T3.1. |
| T3.5 | Play the original game | Decided, not started | An option in Options > Game that restarts into GX as shipped, every change of ours off except high refresh and ray tracing. Hidden until the whole game runs natively (2026-11-15). |

#### T4: Linux

| ID | Item | Status | Notes |
|---|---|---|---|
| T4.1 | The program builds and runs on Linux | Partly | One source tree builds both. On Linux it runs without a window so far (tests, the online race against Windows in section 1). |
| T4.2 | A window, sound and pads on Linux | Not started | |
| T4.3 | Ray tracing on Linux | Not started | Ray tracing uses Direct3D 12; on Linux the setting is shown disabled until a Vulkan ray path exists (2026-11-10). |
| T4.4 | Linux as a release target | Decided | In 0.7 (2026-11-14). Linux results stay bit-exact with Windows (P14). |

#### T5: Every place its own object

Each race is an object that owns all of its state (memory, heaps, sound voices, graphics resources); the Garage and
the menu own theirs; handles carry their owner. This is how D3 is reached. Decided 2026-11-15.

| ID | Item | Status | Notes |
|---|---|---|---|
| T5.1 | Owners and handles | Done | |
| T5.2 | The Garage as its own object | Done | The menu never writes a race's memory. |
| T5.3 | Races own all their state | Not started | Designed. |
| T5.4 | Every place its own instance | Not started | Designed for any number of objects. Limits planned for: memory per race, one heard race at a time, an online race's step never delayed by the others. |

#### T6: The picture

| ID | Item | Status | Notes |
|---|---|---|---|
| T6.1 | High refresh | Done | The recompiled game's run-ahead through the shared core (P15): real frames, up to 240 a second. |
| T6.2 | True widescreen, resolution, anti-aliasing | Done | Applied while running (M1.2). |
| T6.3 | Ray tracing | Done (Experimental) | Switched on and off while running. Labelled Experimental until its known faults are fixed (L-RT). |

#### T7: The rolling start

| ID | Item | Status | Notes |
|---|---|---|---|
| T7.1 | The rolling start chosen on 2026-11-13 | Decided | Through the countdown each machine follows one of the course's four racing lines by its grid place (1st on line 1, 2nd on line 2, 5th on line 1 again); from GO every machine is on its own line and the players drive. The grid is placed so the leader crosses the line at about GO, with the placing fixed for split roads. Planned for the release after 0.6.5. Whether it is in the native program yet is not established. |

## 5. Long term

Planned, not scheduled, unless a date is given. Items depend on 0.7 unless said otherwise.

### 5.1 The four loops and more queues

The order is decided: Time Attack first, then the daily cup, then Create, then ranked seasons (2026-11-13). Series
and Grand Prix online (L-SERIES) come at about the same time as Time Attack and accounts.

#### L-TA: Time Attack

- **What:** leaderboards for each course, ghosts, times you can trust.
- **Status:** not started.
- **Decided:**
  - Time Trial rules are pure vanilla and not adjustable: the stock rules exactly (2026-10-31).
  - Separate boards by category (maximum speed, maximum acceleration, spaceflying).
  - Players can race a friend's ghost, drawn from the friend's replay (2026-11-08).
  - Times are set offline. A time reaches a board only after a **verifier** has re-run its inputs and measured the
    same result. Verifiers are headless programs run by trusted people with their own disc, each with a revocable
    key. The online service never holds game data (2026-10-28).
  - Board runs come from a mode without rewind.
- **Depends on:** L-ACC, T3.1.

#### L-SERIES: Series and Grand Prix online

- **What:** races grouped into a series across several courses, scored over the whole series, as the game's Grand
  Prix is, in lobbies and as modes.
- **Status:** not started. Planned for about the same time as Time Attack (L-TA) and accounts (L-ACC).
- **Decided:** a series scores with the game's own Grand Prix points by place (2026-10-29).
- **Depends on:** L-MODE (the scores a mode keeps across races).

#### L-CUP: Daily cup

- **What:** a scheduled event: time-trial qualifying that sorts players into divisions of 30, then each division
  races Knockout.
- **Status:** not started. Knockout and 30-player lobbies exist.
- **Depends on:** L-TA, L-ACC.

#### L-CREATE: Create

- **What:** a track editor in the game; tracks shared by code; a Track of the Day; mode files shared as rulesets.
- **Status:** not started in the game. A separate stage editor exists outside the game.
- **Decided:** a shared track carries no disc textures: it is the course file, its own models and a list of the disc
  textures it uses, which each player's program takes from their own disc.
- **Open:** racing custom tracks online.
- **Depends on:** T5, M6.

#### L-RANK: Ranked seasons

- **What:** a visible rank and seasons.
- **Status:** not started.
- **Decided:** ranked play needs stored match records, which need the account database.
- **Depends on:** L-ACC.

#### L-QUEUE: More queues

- **What:** Quick Play is the first queue. Later, an Arcade in the manner of Overwatch's: other playlists of modes,
  each its own queue.
- **Status:** not started.

### 5.2 Watching and tournaments

#### L-SPEC: Advanced spectating

- **What:** join a race mid-way by catching up as a rejoin does; a spectator key on the relay (receives, sends
  nothing, out of the checksum vote); follow any racer; camera kinds; our minimap and standings; scrub back and jump
  to live.
- **Status:** partly. Spectating exists in 0.6.5; natively, M4.12.
- **Decided:**
  - Built before the control room.
  - A racer who was knocked out can follow racers and change cameras but cannot scrub.
  - The observer HUD cycles Observer, Game and Clean.

#### L-TOUR: Tournaments and the control room

- **What:** for organisers, built on L-SPEC: several views, a stream delay, a director, overlays sent to OBS
  (Spout2).
- **Status:** not started.
- **Decided:** a desynced tournament player may come back by rejoining with a fresh game re-simulated from the
  relay's inputs (nothing repaired, P10).

#### L-TV: Piconet TV

- **What:** an always-on broadcast of Quick Play races with nobody at the controls: a watching game with a camera
  director and an overlay in Direction D. Custom lobbies are not broadcast; lobby chat never goes on stream; player
  names show in the results.
- **Status:** designed, not released.
- **Decided:** players see a recording light on the lobby page while the broadcast watches that lobby.
- **Depends on:** L-SPEC, T4.

### 5.3 Accounts and social

#### L-ACC: Accounts

- **What:** optional sign-in for leaderboards, friends, real stats, stored emblems, and online storage for sharing.
- **Status:** not started.
- **Decided (2026-10-15):**
  - Sign-in is optional; racing never needs it.
  - Two ways: Discord (identify only) and passkeys. No passwords, no email.
  - One account can carry both methods.
  - Nothing is stored about players who are not signed in.
  - Profile links use the player's name and tag. Tags are derived, never chosen.
- **Decided (2026-11-16): storage and sharing.**
  - An account signed in with Discord comes with online storage.
  - Players use it to save replays online and to upload and share replays, ghosts, courses, textures, models and
    other things they make.
  - What is uploaded is the player's own work: game data from the disc is never uploaded (as for shared courses,
    L-CREATE).

#### L-SOC: Friends and social

- **What:** Friends and Recent in Social, online status.
- **Status:** not started (the sections exist, switched off).
- **Decided:** no global list of who is online until players can hide their status.
- **Depends on:** L-ACC.

### 5.4 In the race

#### L-MAP: Our own 3D minimap

- **What:** the game's minimap does not suit 30-machine races and is never drawn. Ours: simple coloured dots, fairly
  small; three bigger dots for you, the leader and your rival; the game's yellow start mark; the C-stick turns it.
- **Status:** partly (in the recompiled game; not yet native).

#### L-HUD: The game's HUD, moved by modes

- **What:** parts of the game's own HUD placed by a mode file (`hud_move`); added parts drawn in the game's style.
- **Status:** in 0.6.5; to be carried into native.
- **Decided:** no automatic HUD defaults; the mode file places the parts. The separate modern HUD was dropped
  (I-MHUD).

#### L-SPLIT: Split screen

- **What:** several local players on one race, each view its own full picture. Layouts: separate windows (the
  default), stacked, side by side, all one system. Online, a second player has a guest name.
- **Status:** prototype in the recompiled game, not released; not native.
- **Decided:** a second view must not cost double.

#### L-AI: An AI racer

- **What:** a racer for any course, custom ones included, with one difficulty knob.
- **Status:** paused (2026-11-05). A first version drove Twist Road laps of 30.6 s and 28.6 s, against the game
  CPU's 27.3 s and 20.6 s.
- **Decided:** a hand-written network (weights written, not trained); fixed-point so results are identical on x86
  and ARM; its weights' hash recorded in replays; a standalone program; offline only.

#### L-RULES: Combat and wall rules

- **What:** mode parameters for Damage, Knockback, Wall damage and Wall bounce (10% to 1000%), spin and side attacks
  on or off, spin-attack immunity.
- **Status:** designed; whether all of it is in a release is to be checked.
- **Decided:** defaults are the game's own, bit-identical (P3).

#### L-FIX: Known rule fixes waiting

- **Rule times in real seconds:** a race runs at 59.94 steps a second, but rule times count 60, so a mode's 10 s
  lasts 10.01 s. To be fixed behind a version switch so older replays keep the old count.
- **A lone player out ends an offline race at once:** decided to be fixed so the race runs on for the CPUs. Not in
  0.6.5.

### 5.5 Game modes

#### L-MODE: The game mode system

- **What it is (built):** modes are text files in a small language, compiled by the protocol's compiler, run inside
  the race step and recorded in the replay. Quick Play's rotation is a list of mode files; hosts upload their own in
  custom lobbies. A mode file names the game version it was written for (`api 0.6.5;`). Each released set of words
  is frozen; newer games run every older mode, never the other way round.
- **Decided:**
  - Nothing is hard-coded but the vocabulary: the program provides what a mode can read and trigger; everything else
    is defined in mode files.
  - A word left out means the game's own behaviour (P3).
  - Lobby settings are limited to those listed under M5.
  - Explaining modes to new players beyond the vote card's flip side was set aside (2026-11-12).
- **Open words** (not started): placing a machine (a re-grid; an experiment exists), laps and distance, race and lap
  time slots, the KO count, a second countdown, FINISH! or RETIRED on the mode's word, camera, sound.

#### L-PACK: Balance sets per lobby

- **What:** community balance mods as stat packs: a table of changed values, including the code constants a pack
  may carry, handed out by the server, recorded in replays, voted in Quick Play.
- **Status:** in 0.6.5 with GX Unleashed (by AKC12 and Twilight) and F-Zero GX+ (by Emarcee); native: M5.5.
- **Planned:** mixed packs per seat, the lobby deciding whether mixing is allowed.

### 5.6 Making things

#### L-PLUG: Plugins, practice tools and the Lab

- **What:** a way to create plugins for the game, for advanced practice and TAS work in the spirit of BizHawk. Lua
  is one option being considered.
- **Status:** not started.
- **Decided:**
  - Practice stays simple; overlays are shared by every mode; a separate **Lab** holds frame advance, input history,
    a piano roll, branches and sweeps.
  - Online, no plugin code runs in the game; only read-only panels of what the game already shows.

#### L-STRUCT: Free native structs

- **What:** moving race state from the game's memory layout to plain native data structures.
- **Why:** clean code, a clean modding interface, and layout limits (P8). Never sold as speed.
- **Status:** not started; after the first native release (0.7-e). T5 comes first.
- **Binding:** anything a race reads changes layout only once P2's equality proves it.

#### L-DECOMP: The decompilation

- **What:** [karamzov123/fzero-gx-decomp](https://github.com/karamzov123/fzero-gx-decomp), which the native program
  is built from.
- **Status:** complete: every function matches the retail game byte for byte. Piconet contributed matched functions
  to it (the first, pull request #159).

### 5.7 Technology

#### L-RT: Ray tracing as a finished feature

- **Goal:** what an F-Zero made in 2026 would look like: modern effects that keep the original look. Judged in
  motion at race speed.
- **Status:** Experimental (T6.3).
- **Decided:**
  - Materials are derived from each model's own data, never a list per model, so custom models work.
  - Lights match the game's own sources one to one (three burners, three lights); tags identify, never restyle.
  - The game's culling and level of detail stay as they are; the ray scene is built at load.
  - Rays are picture-only: players with and without them race together.
  - Linux needs a Vulkan path (T4.3); macOS later.

#### L-240: Real high refresh

- **Rule:** high refresh means real frames from run-ahead, never warp or frame generation. Done (T6.1); further work
  is pacing.

#### L-ARM: An ARM port

- **What:** builds for ARM that race x86 players with identical results.
- **Status:** not started. Every change keeps it possible (P16).

#### L-CONSOLE: Replays for the console's viewer

- **What:** converting a Piconet replay of an offline race into a replay file the stock game's viewer reads (P1).
- **Status:** not started. A console netplay client was rejected.

## 6. Ideas, not planned

None is to be started without the item being reopened on the Discord.

| ID | Idea | State | Note |
|---|---|---|---|
| I-E2EE | End-to-end encrypted chat | Parked (2026-11-02) | MLS would be the standard. Open: what it protects against, and moderation. |
| I-MUSIC | A music player | Largely realised | Course & Music (T1.6). |
| I-PILOT | Swapping pilots between machines | Parked, low priority | Drawing only. |
| I-LAB | A technique lab | Idea | Search input variations in the exact simulation and flag anything faster than normal driving, for a human to learn. |
| I-FILTER | A filter language for lists | On hold | One query box, for Replays first. |
| I-BOT | A Discord bot | Paused (2026-11-04) | |
| I-TEX | HUD texture packs | Parked | |
| I-200 | Races of up to 200 players | Shelved | The game is built for 30 machines; would need L-STRUCT. |
| I-CATCHUP | A smoother catch-up at the race start | Parked | |
| I-MHUD | A modern HUD of our own | Dropped (2026-11-04) | Replaced by L-HUD. |
| I-WHEEL | Steering wheels in online races | Parked | |
| I-VOTE | One vote system for any number of votes | Idea | |
| I-LANES | Racers as lanes under the replay bar | Paused | |
| I-REGION | Regions with a live ping each; lobbies by region | Idea | |
| I-REPORT | Bug reports sorted automatically, with Discord forum threads | Idea | |
| I-MCORE | Physics on several cores | Parked | If it returns, it uses the one rollback library (P12). |
| I-LIGHT | A ray-tracing light cache | Shelved | |
| I-SWAP | A mode word that swaps a machine's model mid-race | Idea | |

## 7. How this file is kept

- It is updated when a milestone's status changes, when a decision is taken or reversed, and when an item lands: the
  row, its note and the date at the top. Changes go through a pull request like any other.
- A decided item records its date. A reversed decision is replaced, not kept beside the new one.
- An item's ID never changes and is never reused. A dropped item moves to section 6 with its state.
- Status is checked against the code, not remembered. When unsure, an item says Partly and what is known.
