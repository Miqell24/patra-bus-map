# patra-bus-map

Interactive web map of the Patra city buses — **30 lines of Αστικό ΚΤΕΛ
Αχαΐας** — in the visual logic of a classic printed network map: every route
drawn exactly along the streets it takes (own HMM/Viterbi map matching on the
OSM road graph), line numbers written parallel to every street they use,
labeled stops and terminus boxes.

**Live map:** https://agcghub.github.io/patra-bus-map/

A sibling of [volos-bus-map](https://github.com/AGCGHub/volos-bus-map) and
[larisa-bus-map](https://github.com/AGCGHub/larisa-bus-map): the same
pipeline, the same data platform.

## The feed

Patra publishes no GTFS — not on data.gov.gr, not in MobilityDatabase or
Transitous. The operator's passenger site `patra.citybus.gr` runs on the
citybus.gr platform (agency 112), and its REST backend carries the whole
network: every pole, every line with its patterns, each pattern's stop
sequence and road polyline. `pipeline/citybus-feed.mjs` reads the short-lived
token the page hands every visitor and writes `data/gtfs/` out of it: 30 lines,
62 patterns, 817 poles (11.09.2026). One trip per pattern — the endpoint
publishes patterns, not runs — so the build draws every pattern
(`allVariants`).

**Since the renumbering a number is a direction.** Most lines run one way and
the way back is a line of its own: 101 goes out to Εγλυκάδα, 104 comes back.

## Stop names

The operator writes in capitals without accents, partly in lower case without
accents, and spells the dialytika with apostrophes (ΑΧΑ'Ι'ΑΣ). The build turns
`'Ι'`/`'Υ'` into Ϊ/Ϋ, recovers accents from an OSM name dictionary of the frame
(`lib/greek.mjs`) plus a seed of common words, and leaves surnames it cannot
verify as the operator spells them.

## Usage

```bash
npm run download   # citybus.gr → data/gtfs, OSM tiles from ../_pbf/greece-latest.osm.pbf, MapLibre
npm run build      # map matching → data/out/
npm run lines      # the per-line ("Lines") view
npm run serve      # http://localhost:8195
```

On Windows `pipeline/pbf-tiles.py` needs `PYTHONUTF8=1`.

Known gap: the operator's polyline for 101 crosses the pedestrian Μαιζώνος in
the centre (about 200 m drawn off the road graph).

## Data attribution

Timetables: Αστικό ΚΤΕΛ Αχαΐας Α.Ε. (patra.citybus.gr) · map data ©
OpenStreetMap contributors · tiles by OpenFreeMap.
