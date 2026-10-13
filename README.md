# Simba-Farming

AIO farming script / plugin for Simba + WaspLib (OSRS).

## Patch transport

Internal to the farm run handler. Data in `data/travel.json` (version 2). Transport only gets you **within walking distance** of a farm place; `WalkFind` + the walker take over after that.

```text
places[]    farm chunk areas: map (Map.AddEx), local (skip-tele), default methods
methods[]   transport options: type, place, landing [x,y,radius], fields, reqs
patches[]   patch id → place (for future GUI config)
```

Farm run chooses the transport method per patch, then:

```pascal
FarmingTravel.Init();
FarmingTravel.Travel(patch.TravelMethod); // one entry point
// → WalkFind / walker / handle patch
```

`Travel`:
1. Load place map chunks  
2. If already in `place.local` → skip teleport (e.g. Catherby allot N → S)  
3. Else run the method and confirm `landing [x,y,radius]`  

### Item reqs

AND of OR-groups on each method (future setup GUI). Runtime ignores them.

```json
"reqs": [
  ["Construct. cape", "Teleport to house", "Law rune"],
  ["Dramen staff", "Lunar staff"]
]
```

### Adding a transport option

1. Ensure the place exists (`map` / `local` / `methods`).
2. Add a method with `place`, `landing`, type fields, optional `reqs`.
3. Append the method id to `places[].methods`.
4. Patches only need `{ "id", "place" }`.

POH handlers are stubbed. Landing centers are placeholders (map-box centers, radius 48) until calibrated in-game.
