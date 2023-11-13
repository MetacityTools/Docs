---
description: ⚠️ in dev - Configuration of Dockerized Metacity Processor
---

# Dockerized Processor

Example config:

```json5
{
    "input": "/dataset/directory",
    "output": "/put/results/here",
    "simplify": false, //optional, default false
    "move_to_plane_z": 0, //optional, default null
    "map_to_layer": "/the/other/dataset/, //optional, default null
    "tree": { //optional, default {}
        "max_depth": 10, //optional, default 10
        "tile_level": 5, //optional, default 5
        "aggregate_mode": "MAX_AREA" //optional, default "MAX_AREA", values ["AVERAGE", "MAX_AREA"]
        "merge_tiles": false, //optional, default false
        "keep_keys": ["usage"], //optional, default null
    }
}        
```
