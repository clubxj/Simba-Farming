# Simba-Farming

AIO farming script / plugin for [Simba](https://github.com/Villavu/Simba) + WaspLib (OSRS).

## Layout

| File | Role |
|------|------|
| `farming_types.simba` | Patch / compost enums |
| `patch_data.simba` | Empty uptext, soil colors, world coords |
| `patch.simba` | Patch find + state machine + actions |
| `travel.simba` | Travel methods to each farm |
| `tool_leprechaun.simba` | Tool leprechaun UI + noting |
| `farm_run.simba` | Multi-patch run + bank withdraw |
| `utils.simba` | Shared helpers (click clustering, finders, spellbook swap) |

## Status

- Hosidius allotment-N is the furthest-along vertical slice (travel + find + harvest/plant/compost/note).
- Hosidius herb / flower / allotment-S coordinates are estimated — calibrate in-game before relying on them.
- Patches after Hosidius in `ERSFarmPatchDescriptor` raise `Patch not yet supported` until `FARM_PATCH_META_DATA` is extended.
- POH portal / nexus travel still raises intentionally.

## Debug

Define `FARM_DEBUG_PATCH_FIND` to draw find boxes on the main screen.
Define `FARM_PATCH_MAIN` at the top of `patch.simba` to run the Hosidius allotment test harness.
