---
description: >-
  Processing the GTFS files before mapping the public transit to the MATSim
  network
---

# Filtering GTFS

## Keeping urban routes only

1. Using R to filter out routes that are suburban and keep only:
   * Tram L1-26, night tram L90-L96
   * Bus L?-L300, night bus L900-L916
   * Subway L991-L993 (metro A-C)
   * Funicular L49
   * Ferry L1801-L1807 (P1-P7)
2. Beware that R cannot process GTFS overflow time (e. g. 25:30)
   * Can be post-processed via Python (NaN values)
