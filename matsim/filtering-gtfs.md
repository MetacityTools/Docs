---
description: >-
  Processing the GTFS files before mapping the public transit to the MATSim
  network
---

# Filtering GTFS

## URL

**Data reference:** [https://developers.google.com/transit/gtfs/reference ](https://developers.google.com/transit/gtfs/reference)

**PID GTFS:** [https://pid.cz/o-systemu/opendata/](https://pid.cz/o-systemu/opendata/)

## Keeping Urban Routes Only

Filter out routes that are suburban and keep only:

* Tram L1-26, night tram L90-L96
* Bus L100-L299, night bus L900-L916
* Subway L991-L993 (metro A-C)
* Funicular L49
* Ferry L1801-L1807 (P1-P7)

## Running the Filter

1. Clone the Metacity-MATSim repository
2. Move into the `utility/gtfs` folder
3. Add the downloaded GTFS folder into the `utility/gtfs/data` folder
4. Run: `python filter_by_route_id.py`
5. The filtered GTFS will be in the `utility/gtfs/output` folder by default
