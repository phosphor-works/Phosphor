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

The repository has no code yet. The design is in PlasmaZones: `docs/phosphor-compositor-core/` (from
`00-index.md` and `01-architecture.md` to the phase chapters) and `docs/phosphor-compositor-core-plan.md`. It still
uses the old `org.plasmazones` names, which become `org.phosphor_works`. Phase 8 of the Phosphor Split Plan moves it
here.

### The design

- Phosphor links phosphor-libs and phosphor-engine-libs, and is rebuilt when their ABI changes.
- It implements the engine runtime's host interfaces itself: `ICompositorBridge` as direct calls into the scene,
  overlays as scene nodes, idle from its own input, shortcuts as its own `PhosphorShortcuts::IBackend`, and its own
  config path through `PhosphorConfig::IBackend`.
- In its own session it owns `org.phosphor_works.Placement`, the public placement API that Facet and scripts call.
  `plasmazonesd` starts only under KWin, so the two hosts never share a session.
- The runtime runs on Phosphor's Qt event loop and thread.

## Layout

There is no code yet, so there is no layout to describe.

## Build and test

There is no build, no CI and no tests yet.

## Rules

### When code lands

Add the layout, the build and test commands and the CI to this file. Phosphor is an app: its code is
`GPL-3.0-or-later`. Add it to the workspace superbuild with a `phosphor` preset.
