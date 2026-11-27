<!-- SPDX-FileCopyrightText: 2026 Phosphor Works Team -->
<!-- SPDX-License-Identifier: GPL-3.0-or-later -->

<!-- BEGIN shared: tools/sync writes this block from workspace/sync/claude-header.md. Change it there. -->
# Phosphor

This file has two parts. The rules that every phosphor-works repository follows come first, from the conventions of
the workspace repository, imported below. The sections after them belong to this repository only: what it is, its
layout, how to build and test it, and its own rules, code, skills and agents, and links to the other repositories.
A section here does not repeat a convention. When a section and the conventions disagree, change one of them so
that they agree.

The Claude Code tooling comes from the plugins that `.claude/settings.json` enables and `workspace/tools/setup`
installs. The `phosphor-works` plugin has the command guard, the shared skills (`phosphor-works:build`, `debug`,
`code-audit`, `record-lesson`, `new-repo`), the shared reviewers, and the lesson index, which adds the lessons that
fit each prompt.

@../workspace/claude/conventions.md
<!-- END shared -->

## Overview

Phosphor is the Wayland compositor of the phosphor-works suite. It links `phosphor-engine-runtime` (the PlasmaZones
daemon as a library, in phosphor-engine-libs) and runs it in its own process, with direct calls and no D-Bus between
them. The app and repository are `Phosphor`, and its package is `phosphor`.

### State

The repository has no code yet. The plans are workspace issues:

- workspace#30: how Phosphor uses the engine runtime, the decisions, the phases, the session and the protocols.
- workspace#66: rendering, GPU, sync, the backends and the nested mode.
- workspace#69 to workspace#72: the library work that Phosphor needs (screens, overlays in the compositor process,
  the Placement D-Bus adaptors, the compositor side of placement).

The first design (`docs/phosphor-compositor-core*`) is only on the PlasmaZones branch `feat/engine-data`. It is
history. Where it disagrees with the issues, the issues are correct.

### The design

- Phosphor bundles its private copies of the phosphor libraries, built from the commits that it pins, and is
  rebuilt when their ABI changes. It uses private Qt API, so it works only with the Qt patch release that it was
  built against.
- It gives the engine runtime the seams of `EngineRuntime::Platform` itself: an in-process layer-shell transport
  that renders the overlays offscreen as scene nodes, the presentation host, panels from the layer-shell exclusive
  zones, its workspaces, idle from its own input, and its own `PhosphorShortcuts::IBackend`. It does not give an
  `IOverlayService`: the runtime makes its own. workspace#30 has the full list.
- The compositor side of placement (drag, snap, tiling, navigation) is the work that the KWin effect does under
  KWin. Phosphor does it with direct calls to the runtime services (workspace#72).
- In its own session it owns `org.phosphor_works.Placement`, the public placement API that Lambent, Facet and
  scripts call. It owns the name before it starts any client. `plasmazonesd` starts only under KWin, so the two hosts
  never share a session.
- The runtime runs on Phosphor's Qt event loop and main thread.

## Layout

There is no code yet, so there is no layout to describe.

## Build and test

There is no build, no CI and no tests yet.

## Rules

### When code lands

Add the layout, the build and test commands and the CI to this file. Phosphor is an app: its code is
`GPL-3.0-or-later`. Add it to the workspace superbuild with a `phosphor` preset.
