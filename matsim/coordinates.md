---
description: Description of used coordinates in MATSim simulation
---

# Coordinates

## Prague / Czechia

#### Coordinates: S-JTSK + Bpv (YXH) (EPSG:5513, EPSG:2065)

[https://geoportal.cuzk.cz/(S(f11gyyeko4ufnsjy13t1nwz1))/INC/TextMeta/html/CZ/help/transformace/index.html](https://geoportal.cuzk.cz/\(S\(f11gyyeko4ufnsjy13t1nwz1\)\)/INC/TextMeta/html/CZ/help/transformace/index.html)

#### Why EPSG:5513 (and not EPSG:5514)?

[https://osgeo-org.atlassian.net/browse/GEOT-5865](https://osgeo-org.atlassian.net/browse/GEOT-5865https://gis.stackexchange.com/questions/321612/geotools-crs-transformation-for-epsg5514-to-wgs84-does-not-work)

[\
https://gis.stackexchange.com/questions/321612/geotools-crs-transformation-for-epsg5514-to-wgs84-does-not-work ](https://osgeo-org.atlassian.net/browse/GEOT-5865https://gis.stackexchange.com/questions/321612/geotools-crs-transformation-for-epsg5514-to-wgs84-does-not-work)



## MATSim EPSG:5513

Matsim cannot comprehend negative axis values while projecting long-lat to x-y.

**Result: flipped axes!!** MATSim's 5513: x = 7...., y = 10...



_**Official Krovak EPSG:5514 (is negative): \[x,y]**_

In relation to official Krovak:

* EPSG:5513 (is positive): \[-y, -x]
* MATSim's EPSG:5513: \[-x, -y]
