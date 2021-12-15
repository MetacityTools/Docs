---
description: How to preprocess OSM before creating a MATSim network
---

# OSM

## Data Reference

[http://download.geofabrik.de/osm-data-in-gis-formats-free.pdf](http://download.geofabrik.de/osm-data-in-gis-formats-free.pdf)

## [Planet OSM](https://planet.openstreetmap.org)

1. Download Czech Republic (.osm): [https://download.geofabrik.de/europe/czech-republic.html](https://download.geofabrik.de/europe/czech-republic.html)&#x20;
2. Use [osmium-tool](https://osmcode.org/osmium-tool/) to extract Prague: [https://osmcode.org/osmium-tool/manual.html#the-osmium-command](https://osmcode.org/osmium-tool/manual.html#the-osmium-command)&#x20;
   1. Polygons describing Prague (.geojson) - 57 districts: [https://www.geoportalpraha.cz/en/data/opendata/E9E20135-18B3-4163-B516-45613956B856](https://www.geoportalpraha.cz/en/data/opendata/E9E20135-18B3-4163-B516-45613956B856)&#x20;
   2. Join the districts into one polygon: `./dissolve-polygon.py MAP_MESTSKECASTI_P.json prague-polygon.json`
   3. Run: `osmium extract -p prague-polygon.json czech-republic-latest.osm.pbf -o prague.osm.pbf`
3. _Optional: Create a multimodal network (roads, pt, rails, ...)_\
   __[https://github.com/matsim-org/pt2matsim/wiki/Creating-a-multimodal-network-(Osm2MultimodalNetwork)](https://github.com/matsim-org/pt2matsim/wiki/Creating-a-multimodal-network-\(Osm2MultimodalNetwork\))&#x20;

## [OpenStreetMaps](https://www.openstreetmap.org)

_**Allows to export only a small area!**_

1. Export OSM data (map.osm)
2. Compress using some tool to .osm.pbf (e. g. osmium-tool): [https://osmcode.org/osmium-tool/](https://osmcode.org/osmium-tool/)&#x20;
3. Use SupersonicOsmNetworkReader to change data to MATSim XML
   1. Or deprecated [https://www.matsim.org/apidocs/core/0.4.0/org/matsim/core/utils/io/OsmNetworkReader.html](https://www.matsim.org/apidocs/core/0.4.0/org/matsim/core/utils/io/OsmNetworkReader.html)&#x20;
4. Run NetworkSimplifier from MATSim - improves simulation performance, introduces artifacts
5. Run NetworkCleaner from MATSim
