# Simba-Farming

AIO farming script / plugin for Simba + WaspLib (OSRS).

## Patch transport (JSON)

All farm travel is data-driven from `data/travel.json`.

```text
destinations[]  map boxes for arrival confirmation
methods[]       how to travel (item / magic / poh_* / confirm)
patches[]       which methods are valid for each patch id
```

### Usage

```pascal
FarmingTravel.Init();                              // loads data/travel.json
FarmingTravel.Travel('xeric_talisman_glade');      // one method
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N');  // try patch method list
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N', 'hosidious_tablet');
```

`TRSFarmPatch.TravelMethod` is a method id string. Leave it empty to use the patch route list from JSON.

### Adding a new travel option

1. Add a destination map box if needed.
2. Add a method with `type`, `destination`, and type-specific fields (`item`/`option` or `spell`).
3. Append the method id to the relevant `patches[].methods` arrays.

POH portal / nexus / spirit-tree handlers are stubbed and raise until implemented.
