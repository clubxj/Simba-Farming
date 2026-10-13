# Simba-Farming

AIO farming script / plugin for Simba + WaspLib (OSRS).

## Chunks

`data/chunks.json` + `chunks.simba` own map load areas in **WaspLib chunk coordinates** (256×256 map tiles), not raw world pixels.

```json
{ "id": "catherby", "chunks": [26, 142, 29, 143], "layers": [0] }
```

`chunks` is a chunk-space box passed to `Map.AddEx` (same style as WaspLib `Box(45, 51, 46, 50)`). Y may be inverted; the engine expands the box to the chunk grid.

```pascal
FarmingChunks.Init();
FarmingChunks.LoadMap('catherby');
FarmingChunks.ContainsPlayer('catherby'); // skip-tele / already here
```

Chunk boxes in-repo were bootstrapped from older world boxes via `Expand(80)` + ÷256. Verify in-game with `RSTranslator.Global2Chunk` and edit `chunks.json` as needed.

## Patch transport

Internal to the farm run handler. `data/travel.json` holds **methods only**.

```text
methods[]  id, type, chunk, landing [world x,y,radius], type fields, reqs
```

Farm run chooses the transport method per patch, then:

```pascal
FarmingTravel.Init();                 // also inits chunks if needed
FarmingTravel.Travel(patch.TravelMethod);
// → WalkFind / walker / handle patch
```

`Travel`:
1. `FarmingChunks.LoadMap(method.chunk)`
2. If player chunk is inside that area → skip teleport
3. Else run the method and confirm `landing`

### Item reqs

AND of OR-groups on each method (future setup GUI). Runtime ignores them.

```json
"reqs": [
  ["Construct. cape", "Teleport to house", "Law rune"],
  ["Dramen staff", "Lunar staff"]
]
```

### Adding a transport option

1. Ensure the chunk exists in `chunks.json`.
2. Add a method in `travel.json` with `chunk`, `landing`, type fields, optional `reqs`.

POH handlers are stubbed. Landing centers are placeholders until calibrated in-game.
