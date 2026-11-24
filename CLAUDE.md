<!-- SPDX-FileCopyrightText: 2026 fuddlesworth -->
<!-- SPDX-License-Identifier: GPL-3.0-or-later -->

# Phosphor

Phosphor is the Wayland compositor of the phosphor-works suite. It links `phosphor-engine-runtime` (the PlasmaZones
daemon as a library, in phosphor-engine-libs) and runs it in its own process, with direct calls and no D-Bus between
them. The app and repository are `Phosphor`, and its package is `phosphor`.

The rules every phosphor-works repository follows come from the workspace repository, imported below. This file adds
Phosphor's own. The shared Claude Code tooling (the command guard, `phosphor-works:build`, `phosphor-works:code-audit`,
the shared reviewers, the lesson index) comes from the `phosphor-works` plugin, which `.claude/settings.json` enables
and `workspace/tools/setup` installs.

@../workspace/claude/conventions.md

## State

The repository has no code yet. The design is in PlasmaZones: `docs/phosphor-compositor-core/` (from
`00-index.md` and `01-architecture.md` to the phase chapters) and `docs/phosphor-compositor-core-plan.md`. It still
uses the old `org.plasmazones` names, which become `org.phosphor_works`. Phase 8 of the Phosphor Split Plan moves it
here.

## The design

- Phosphor links phosphor-libs and phosphor-engine-libs, and is rebuilt when their ABI changes.
- It implements the engine runtime's host interfaces itself: `ICompositorBridge` as direct calls into the scene,
  overlays as scene nodes, idle from its own input, shortcuts as its own `PhosphorShortcuts::IBackend`, and its own
  config path through `PhosphorConfig::IBackend`.
- In its own session it owns `org.phosphor_works.Placement`, the public placement API that Facet and scripts call.
  `plasmazonesd` starts only under KWin, so the two hosts never share a session.
- The runtime runs on Phosphor's Qt event loop and thread.

## When code lands

Add the layout, the build and test commands and the CI to this file. Phosphor is an app: its code is
`GPL-3.0-or-later`. Add it to the workspace superbuild with a `phosphor` preset.
