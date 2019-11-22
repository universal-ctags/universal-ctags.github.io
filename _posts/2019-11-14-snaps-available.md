---
layout: post
title: Snaps are available.
---

Universal-ctags has long been available as a snap. For a short while,
the snap wasn't actively being maintained, but now it has an active
maintainer again.

Recent snap fixes include adding an automatically applied alias of 'ctags' when
the snap is installed, adding an end-to-end test to check the snap builds and
installs a working application, and adding CI using Travis.

Applications installed as a snap are subject to security restrictions,
which can introduce limitations compared to installing as an apt
or compiled from source.

Our snap's limitations are now documented on
[universal-ctags' page in the snapstore](https://snapcraft.io/universal-ctags),
and the new snap maintainer is working to reduce or eliminate them.

Currently the biggest limitation is that a project's `.ctags.d/` config
directory cannot be read from the current directory. This can be worked around
by moving it to a non-hidden directory name, and using `--options` to
explicitly tell ctags to use it. The user's `~/.ctags.d/` directory is read as
expected.

Otherwise, the snap is viable for daily use,
and is currently installed by over 500 users and growing.

If you have any thoughts on those limitations,
or on other issues you find with the snap,
[let us know](https://github.com/universal-ctags/ctags-snap/issues).

