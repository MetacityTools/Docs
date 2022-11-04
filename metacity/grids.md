---
description: Tiling data for streaming with grids
---

# Grids

Working with large datasets locally might be fine, but for streaming, it is necessary to tile the data into individual tiles and load only what's in the viewport.

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer, Grid, Model
from metacity.io import parse_recursively

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))

<strong>#Grid(self, tile_width: float, tile_height: float) -> Grid
</strong>grid = Grid(1000, 1000)
<strong>#Grid.add_layer(self, layer: Layer) -> None
</strong>grid.add_layer(terrain)
<strong>#Grid.add_model(self, arg0: Model) -> None
</strong>grid.add_model(Model())</code></pre>

### Exporting Data

This is the final step of getting the data ready for visualization. Exporting data from the grid prepares several files for streaming:

<pre class="language-python"><code class="lang-python">from metacity.utils.filesystem import recreate_dir

<strong>#recreate_dir(dir: str) -> None
</strong>recreate_dir("terrain_grid_export")
<strong>#Grid.to_gltf(self, dir: str) -> None
</strong>grid.to_gltf("terrain_grid_export")</code></pre>

The first `recreate_dir` empties the specified directory and  `Grid.to_gltf` then fills it with individual tiles and a `layout.json:`

```
terrain_grid_export
├── layout.json
├── tile123_456.glb
...
└── tile678_879.glb 
```

* The individual tiles are `.glb` files named `tilex_y.glb` where `x` and `y` are replaced by the respective tile coordinates.
* In the examples above, the grid is tiled into tiles with the dimensions 1000 by 1000 units
* The `layout.json` file contains the mapping of the tiles to the real coordinates

The `layout.json` file for this example would look like this:

```json5
{
    "tileHeight": 1000.0,
    "tileWidth": 1000.0,
    "tiles": [
        { //start tile
            "file": "tile-731_-1036.glb", //name of the file
            "size": 174, //number of objects inside a tile
            "x": -731, //tile x-coordinate, 
                       //to get a real coordinate, multiply by tileWidth
            "y": -1036 //tile y-coordinate
                       //to get a real coordinate, multiply by tileHeight
        }, //end tile
        ... //more tiles
    ]
}
```

{% hint style="info" %}
In the example above, the contents of the`terrain_grid_export` directory can be uploaded to a static file server and used as a data source for a [`BananaGL`](../archives/bananagl/) Layer. The library automatically downloads the `layout.json` and operates based on its contents.
{% endhint %}

## Grid Modifiers <a href="#grid-modifiers" id="grid-modifiers"></a>

### Tile Merging&#x20;

Sometimes, a single tile contains a lot of models, but you don't need to distinguish between them; you only care about getting everything rendered quickly. It is advisable to _merge_ all models in individual tiles into one model per tile.

{% hint style="warning" %}
Remember the [`Attribute` type mixing rules](models-and-attributes.md#attribute-caveats)? Similar rules apply here:

* All models must contain identically named `Attributes`
* All `Attributes` with corresponding names across models must be of the same type
{% endhint %}

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer, Grid, Model
from metacity.io import parse_recursively

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))
grid = Grid(1000, 1000)
grid.add_layer(terrain)
<strong>#Grid.tile_merge(self) -> None
</strong>grid.tile_merge()</code></pre>

Now, each tile inside `grid` contains only a single model.&#x20;
