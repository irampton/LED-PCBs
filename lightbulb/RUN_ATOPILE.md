# Running Atopile

This project requires Atopile `^0.15.7`, as configured in `ato.yaml`.

## VS Code Extension Install

The VS Code Atopile extension can provide a managed Atopile CLI even when `ato` is not on `PATH`.

On this machine, the extension-managed Atopile `0.15.7` executable is:

```powershell
$ato = "$env:APPDATA\Code\User\globalStorage\atopile.atopile\uv\cache\archive-v0\4ZmfCNZBwtTZbU1gBhEVb\Scripts\ato.exe"
```

Check the version:

```powershell
& $ato --version
```

## Build

Run from the project directory that contains `ato.yaml`:

```powershell
cd "C:\Code Projects\PCBs\Long Dual White LED Panel\Long Dual White LED Panel"
$env:PYTHONUTF8 = "1"
$env:PYTHONIOENCODING = "utf-8"
New-Item -ItemType Directory -Force ".\build\codex-ato-logs" | Out-Null
$env:FBRK_LOG_DIR = (Resolve-Path ".\build\codex-ato-logs").Path
& $ato build
```

`PYTHONUTF8` and `PYTHONIOENCODING` avoid Windows console encoding errors when Atopile prints Unicode status symbols.

`FBRK_LOG_DIR` keeps this manual run's logs inside the project instead of the VS Code extension's shared log database.

## Known Result On This Machine

The Atopile model currently parses and verifies with Atopile `0.15.7`, then the build exits during post-instantiation setup with Windows code `3221225501` (`0xC000001D`, illegal instruction).

The direct traceback points into the native Faebryk/Zig path:

```text
faebryk.core.node.get_connected
faebryk.core.node.group_into_buses
faebryk.library.is_alias_bus_parameter.resolve_bus_parameters
atopile.build_steps.post_instantiation_setup
```

That failure appears to be native runtime behavior in the extension-managed `0.15.7` environment, not an Ato syntax error.
