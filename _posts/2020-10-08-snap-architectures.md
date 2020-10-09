---
layout: post
title: Snaps now built for more architectures
---

# Snaps now built for more architectures

Linux installs using Snaps added support for more architectures,
including:

    arm64, armhf, i386, ppc64el, and s390x.

The new architectures are released to the 'edge' channel for now.
You can install them on devices of those architectures, such as Pis,
using:

    snap install --edge universal-ctags

File [an issue](https://github.com/universal-ctags/ctags-snap/issues)
if there are any problems, or to tell us that they work fine!

If we see download stats for these versions with no issues reported,
then we'll promote them to the stable channel in a few days.

The existing `amd64` architecture also received the same update, which has been
tested and promoted to the stable channel already. Updates should roll out
imminently to those of our 2,000 existing Snap users who have auto-updates
enabled.

