# Simba-Farming

AIO farming script / plugin for Simba + WaspLib (OSRS).

## Patch transport

Travel is data-driven from `data/travel.json` (version 2). Transport only gets you **within walking distance** of a farm place; `WalkFind` + the walker take over after that.

```text
places[]    farm chunk areas: map (Map.AddEx), local (skip-tele), default methods
methods[]   how to teleport: type, place, landing [x,y,radius], fields, reqs
patches[]   patch id → place (GUI association + TravelToPatch)
```

### Runtime flow

```text
TravelToPatch(patch)
  load place map chunks
  if already in place.local → done (e.g. Catherby allot N → allot S)
  else try place methods (or preferred) until landing confirms
→ WalkFind / walker / handle patch
```

### Usage

```pascal
FarmingTravel.Init();
FarmingTravel.Travel('xeric_talisman_glade');
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N');
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N', 'hosidious_tablet');

// GUI helpers
FarmingTravel.MethodsForPatch('CATHERBY_ALLOT_N');
FarmingTravel.PatchesForPlace('catherby');
```

`TRSFarmPatch.TravelMethod` is a method id string. Leave it empty to try the place’s method list.

### Landing vs local

| Field | Meaning |
|---|---|
| `places[].map` | Chunk load (`Map.AddEx` on expanded box) |
| `places[].local` | Already at farm → skip teleport |
| `methods[].landing` | `[x, y, radius]` — teleport success confirm (jitter) |

Transport does **not** walk from landing to the patch.

### Item reqs

AND of OR-groups on each method (for a future setup GUI). Runtime ignores them.

```json
"reqs": [
  ["Construct. cape", "Teleport to house", "Law rune"],
  ["Dramen staff", "Lunar staff"]
]
```

### Adding a travel option

1. Ensure the place exists (`map` / `local` / default `methods`).
2. Add a method with `place`, `landing`, type fields, optional `reqs`.
3. Append the method id to `places[].methods`.
4. Patches only need `{ "id", "place" }` — methods are inherited from the place.

POH handlers are stubbed until implemented. Landing centers in JSON are placeholders (map-box centers, radius 48) and should be calibrated in-game.
