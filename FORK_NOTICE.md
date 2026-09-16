# About this fork

**This software has been modified.** It is a fork of
[alexbatalov/arcanum-ce](https://github.com/alexbatalov/arcanum-ce), maintained by the
[Clarity](https://github.com/FromChaosComesClarity/Clarity) project, and it does **not**
match upstream. Bugs seen in these builds should be reproduced against an upstream build
before being reported to upstream.

The Sustainable Use License requires that modified copies carry a prominent notice saying so.
This is that notice, and the section below is the complete list of what differs.

## What was changed

Upstream tags no releases and its GitHub Actions artifacts expire after seven days, so there
is no stable URL a game manager can install from. This fork closes that gap, and along the
way carries three community fixes that upstream has not merged.

### Merged pull requests, none of them our work

| Upstream PR | Author | What it fixes |
|---|---|---|
| [#146](https://github.com/alexbatalov/arcanum-ce/pull/146) | [@Corrosion667](https://github.com/Corrosion667) | Falls back to the launch directory when the base path has no game data. Fixes the Apple Silicon `Invalid path "tig.dat"` failure reported in [#145](https://github.com/alexbatalov/arcanum-ce/issues/145). |
| [#157](https://github.com/alexbatalov/arcanum-ce/pull/157) | [@gzenz](https://github.com/gzenz) | Dialogue text scaling and a resizable window. Reserves the option-box band so NPC replies stop overlapping the player's dialogue options. |
| [#158](https://github.com/alexbatalov/arcanum-ce/pull/158) | [@gzenz](https://github.com/gzenz) | An optional FFmpeg backend for Bink playback, so movies play on macOS and Linux at all. See [#28](https://github.com/alexbatalov/arcanum-ce/issues/28). |

All three merged cleanly against upstream `main` and are carried unaltered. The credit for
them belongs to their authors; this fork only builds them.

### Changes made here

- `FORK_NOTICE.md`, this file.
- `.github/workflows/ci-build.yml`: a `workflow_dispatch` trigger so a build can be started
  without inventing a commit, and, in the macOS job, `brew install ffmpeg pkg-config` plus
  `-DARCANUM_BINK_FFMPEG=ON`. That option defaults to OFF upstream, and with it off there is
  no video on macOS at all, because `bink_compat` wraps `binkw32.dll` and that exists only on
  Windows.
- `first_party/bink_compat/CMakeLists.txt`: an `ARCANUM_FFMPEG_LIBDIR` override for the
  directory baked in for `dlopen`. pkg-config on Homebrew reports the build machine's exact
  Cellar revision (`.../Cellar/ffmpeg/9.0.1_1/lib`), which exists on no other Mac, and the
  bare-SONAME fallback does not rescue it on Apple Silicon because macOS does not search
  `/opt/homebrew`. The CI now bakes `/opt/homebrew/lib`, which survives formula revisions.

No other source file has been touched.

## Movies need FFmpeg installed

The FFmpeg libraries are loaded at runtime with `dlopen`, not linked. Without them the game
still starts and simply skips the movies. To get video on macOS: `brew install ffmpeg`.
FFmpeg is used under the LGPL, and no GPL components are involved.

## Licence

arcanum-ce is distributed under the **Sustainable Use License**, which is not an
OSI-approved open source licence. See [LICENSE.md](LICENSE.md) for the full terms. In
particular:

> You may use or modify the software only for your own internal business purposes or for
> non-commercial or personal use. You may distribute the software or provide it to others
> only if you do so free of charge for non-commercial purposes.

Everything built here is published free of charge for non-commercial use, and the licence
terms accompany every release. Clarity **downloads** these builds rather than bundling them,
so that Clarity's own GPL-3.0 terms stay clear of these.

## You still need the game

These builds contain no game data. Arcanum itself must be owned and installed separately,
from [GOG](https://www.gog.com/game/arcanum_of_steamworks_and_magick_obscura) or
[Steam](https://store.steampowered.com/app/500810).
