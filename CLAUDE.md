# Roads Only Map

A single-file web map (`index.html`) that shows Adelaide with just roads, plus place search and a trip planner. No build step, no framework, no API keys.

## Stack
- MapLibre GL JS 4.7.1, loaded from unpkg with fallbacks to jsDelivr and cdnjs (see `loadLibrary()` at the bottom of the file).
- Basemap: CARTO Positron (light) and Dark Matter (dark) vector styles.
- Search: Photon (photon.komoot.io), with Nominatim as a fallback.
- Routing: routing.openstreetmap.de (routed-car, routed-foot, routed-bike).
- State (theme, layer toggles, travel mode, trip stops) is saved in localStorage under `roads-only-map`.

## How the clean-up works
`classify()` sorts every basemap style layer by its source-layer. POIs, house numbers, water labels, borders and rail/transit are always hidden. Road names, suburb and city names, buildings and parks/land are user toggles. `applyLayers()` runs on every `style.load`, so it survives a basemap switch. The trip route layers (`trip-route`, `trip-route-casing`) are re-added in `ensureRouteLayers()` for the same reason.

## Rules for editing
- Keep it one self-contained `index.html` unless there is a good reason to split it.
- Put external text (place names from search) into the page with `textContent`, never `innerHTML`.
- Walk and bike times are computed from fixed speeds (5 km/h, 15 km/h). Driving uses the router's estimate.
- Photon, Nominatim and the OSM routing servers are free community services. Do not add search-as-you-type or anything that sends many requests.

## Status
- Tested in a real browser (Oct 2026): library load, Photon search, OSRM routing (drive/walk), trip stops, theme switch with the route kept, and layer toggles all work.
- CARTO styles now use Protomaps source-layer names (`roads`, `places`, `pois`, `buildings`, `landuse`). `classify()` accepts both these and the old OpenMapTiles names. If a toggle stops working, re-check the style's source-layer names first.
- Not tested: Nominatim fallback, geolocate button, phone layout.
- Deployed: https://dxk-labs.github.io/roads-only-map/ (repo dxk-labs/roads-only-map, GitHub Pages from `main` root). Every push to `main` goes live; the first deploy takes a minute or two.

## Ideas
- Save and name multiple trips.
- Shareable link that encodes the stops.
- Click the map to drop a stop (needs reverse geocoding).
- Opening hours and details for a searched place.
