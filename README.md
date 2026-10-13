# Simba-Farming

AIO farming script / plugin for Simba + WaspLib (OSRS).

## Patch transport

Travel is data-driven from `data/travel.json`.

```text
destinations[]  map boxes for arrival confirmation
methods[]       how to travel (item / magic / poh_* / confirm) + optional reqs
patches[]       which methods are valid for each patch id (tried in order)
```

Handlers live in `travel.simba` (`item`, `magic`, `poh_portal`, `poh_nexus`, `poh_spirit_tree`, `poh_fairy_ring`, `confirm`). Adding a normal teleport is usually a JSON edit only.

### Usage

```pascal
FarmingTravel.Init();                              // loads data/travel.json
FarmingTravel.Travel('xeric_talisman_glade');      // one method
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N');  // try patch method list
FarmingTravel.TravelToPatch('HOSIDIOUS_ALLOT_N', 'hosidious_tablet');
```

`TRSFarmPatch.TravelMethod` is a method id string. Leave it empty to use the patch route list from JSON.

### Item reqs

Each method may declare `reqs`: an AND of OR-groups (arrays of item name strings). Example POH fairy ring:

```json
"reqs": [
  ["Construct. cape", "Teleport to house", "Law rune"],
  ["Dramen staff", "Lunar staff"]
]
```

Loaded onto `TRSFarmTravelMethod.Reqs` for a future setup GUI. Travel runtime does not validate loadouts.

### Adding a travel option

1. Add a destination map box if needed.
2. Add a method with `type`, `destination`, type fields, and optional `reqs`.
3. Append the method id to the relevant `patches[].methods` arrays.

POH handlers are stubbed until implemented.
