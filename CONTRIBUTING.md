# Contributing to SRCDSLab

Thanks for helping! This organization hosts SourceMod plugins, C++ extensions and tools for Source engine servers. Each repository is independent: read its README (and `.github/copilot-instructions.md` when present) before starting.

## Reporting issues

- Use the issue forms and fill in versions and logs: most bugs can't be fixed without them.
- Security problems go through private reporting, see [SECURITY.md](SECURITY.md).
- Questions and support: [Discord](https://discord.gg/EFbtjSUUxG).

## Pull requests

1. Fork the repository and create a branch from the default branch (`master` or `main`).
2. Keep the PR focused on one change, and describe how to test it.
3. Make sure CI is green: it compiles the project and builds the release package.
4. A maintainer will review and merge. Pushes to the default branch publish the `latest` release automatically.

## SourceMod plugins (`sm-plugin-*`)

- Layout: `addons/sourcemod/{scripting,scripting/include,gamedata,translations,configs}`.
- CI compiles with SourceMod **1.12** (`rumblefrog/setup-sp`). Your code must compile there without new warnings.
- Every `.sp` starts with `#pragma semicolon 1` and `#pragma newdecls required`; use new-style declarations and methodmaps.
- Naming: globals prefixed with `g_` (`g_cvFoo`, `g_hTimer`).
- Free handles with `delete`. To reset a `StringMap`/`ArrayList`, `delete` and recreate it instead of calling `.Clear()`.
- Optional dependencies: `#undef REQUIRE_PLUGIN` + `#tryinclude`, then check `LibraryExists` / `GetFeatureStatus` before calling their natives.
- SQL must be threaded (async queries or transactions) with escaped input. Never block the game thread.
- User-facing strings go in `translations/*.phrases.txt`. Create cvars in `OnPluginStart` and call `AutoExecConfig(true)`.
- Bump the version in `myinfo` when behavior changes.

## Extensions (`sm-ext-*`, `mm-ext-*`)

- Built with AMBuild against SourceMod and Metamod:Source 1.12 (see the repository's workflow for exact versions).
- Say which platform and architecture you tested (Linux/Windows, x86/x64).
- Gamedata changes must be verified for every game and platform they touch.

## General

- Many files use CRLF line endings: preserve them so the diff only shows your changes.
- Don't commit build outputs (`.smx`, `.so`, `.dll`) unless the repository already does.
- For forks of upstream projects, consider whether your fix belongs upstream too.
