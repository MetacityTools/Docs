---
description: Tiling data for streaming using quad-trees
---

# Quad-trees

An alternative to [grids](grids.md) is using tree-like structures. Metacity supports exporting data into [quad-trees](https://en.wikipedia.org/wiki/Quadtree):

```python
from metacity.geometry import Layer, QuadTree, MetadataMode
from metacity.io import parse_recursively
from metacity.utils.filesystem import recreate_dir

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))

out_directory = "./terrain-tree-out"
#QuadTree(self, layer: Layer, num_values_mode: MetadataMode, max_depth: int) -> QuadTree
tree = QuadTree(terrain, MetadataMode.MAX_AREA, 10)
recreate_dir(out_directory)
#QuadTree.to_json(self, output_dir_name: str, yield_models_at_level: int, store_metadata: bool = True) -> None
tree.to_json(out_directory, 6, False)
```

### Exporting Data

The example above constructs a quad-tree on top of the layer passed in the constructor and exports it to the `out` directory. The output directory is going to contain the following files:

```
terrain-tree-out
├── meta.json
├── layout.json
├── quad5_123.glb
...
└── quad5_45643.glb
```

The description of the file contents follows:

* `meta.json` contains all data regarding the quad-tree structure, including node metadata
* `layout.json` is a grid-compatible layout (see grid layout above)&#x20;
* `quadx_n.glb` contains geometry for node `n` on the level `x`

The file `meta.json` uses the following structure:

```json5
{
    //present only in the root node, a span of the tree root node 
    "min": [0, 0],
    "max": [100, 100],
    //present only in the nodes where a gltf model has been yielded
    "file": "quad5_345.glb",
    "size": 245,
    //node metadata, aggregated from models contained in the node
    "metadata": {
        "type": "building",
        "xyz": 567
    },
    //north-east node
    "ne": { ... },
    //south-east node
    "se": { ... },
    //south-west node
    "sw": { ... },
    //north-west node
    "nw": { ... }
}
```

### Metadata Processing in Quad-trees

The Quad-tree offers several strategies for dealing with metadata. The model metadata gets processed only if it is a string or a number; other datatypes are currently not supported. &#x20;

There are several modes for dealing with numerical metadata aggregation:

* mode `AVERAGE` (default): averages the numerical values of all models contained in the quad-tree node
* node `MAX_AREA`: uses the value of the model with maximum area

For string-typed attributes, the `MAX_AREA` strategy is always used.

It is possible to filter the metadata:

```python
#QuadTree.filter_metadata(self: QuadTree, attributes: List[str])
tree.filter_metadata(["CTVUK_KOD"])
```

This query leaves only the selected metadata in the tree. This is useful for optimizing the size of the `meta.json`.

## Quad-tree Modifiers

### Tile Merging

Similar to the [grid tile-merging](grids.md#tile-merging) modifier, it is possible to merge all models in a particular tree level:

```python
from metacity.geometry import Layer, QuadTree, MetadataMode
from metacity.io import parse_recursively
from metacity.utils.filesystem import recreate_dir

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))
out_directory = "./terrain-tree-out"
tree = QuadTree(terrain, MetadataMode.MAX_AREA, 10)
recreate_dir(out_directory)

#QuadTree.merge_at_level(self: QuadTree, level: int)
tree.merge_at_level(6)

tree.to_json(out_directory, 6, False) #<- note the export at the same level where the merge was performed 
```

