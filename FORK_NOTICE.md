# About this fork

This is a fork of [alexbatalov/arcanum-ce](https://github.com/alexbatalov/arcanum-ce),
maintained by the [Clarity](https://github.com/FromChaosComesClarity/Clarity) project for one
narrow purpose: to run the upstream CI and publish the macOS build as a durable release.

Upstream has no tagged releases, and GitHub Actions artifacts expire after seven days, so
there is no stable URL a game manager can install from. That is the only gap this fork
exists to close.

## No source modifications

The game's source code is unmodified. This file is the only addition, and it changes nothing
that is compiled or run. If that ever stops being true, this section gets replaced with a
prominent description of what was changed, as the licence requires.

## Licence

arcanum-ce is distributed under the **Sustainable Use License**, which is not an
OSI-approved open source licence. See [LICENSE.md](LICENSE.md) for the full terms. In
particular:

> You may use or modify the software only for your own internal business purposes or for
> non-commercial or personal use. You may distribute the software or provide it to others
> only if you do so free of charge for non-commercial purposes.

Anything built here is published free of charge for non-commercial use, and the licence
terms accompany every release. Clarity **downloads** these builds rather than bundling them,
so that Clarity's own GPL-3.0 terms are not entangled with these.

## You still need the game

These builds contain no game data. Arcanum itself must be owned and installed separately,
from [GOG](https://www.gog.com/game/arcanum_of_steamworks_and_magick_obscura) or
[Steam](https://store.steampowered.com/app/500810).
