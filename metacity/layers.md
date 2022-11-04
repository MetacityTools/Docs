---
description: Organizing things into groups and how to modify them
---

# Layers

Sometimes, it is handy to organize things into groups. In Metacity, you can use `Layers`.

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer, Model
layer = Layer()

models = [Model(), Model()]
<strong>#Layer.add_models(self, models: List[Model]) -> None
</strong>layer.add_models(models)</code></pre>

It is also possible to add a single model:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer, Model
layer = Layer()

model = Model()
<strong>#Layer.add_model(self, model: Model) -> None
</strong>layer.add_model(model)</code></pre>

You can access the individual models again. Deleting a `Model` from the returned list does not remove it from the `Layer`.  The models are not copied, the returned list contains references to the models stored inside Layer.

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
layer = Layer()
#... loading data, processing it

<strong>#Layer.get_models(self) -> List[Model]
</strong>models = layer.get_models()</code></pre>

Moreover, it is possible to store and load the contents of a `Layer` to and from the `.gltf` format.&#x20;

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
layer = Layer()
#... loading data, processing it

<strong>#Store the data
</strong><strong>#Layer.to_gltf(filename: str) -> None
</strong>layer.to_gltf("layer.gltf")

<strong>#Load the data
</strong><strong>#Layer.from_gltf(filename: str) -> None
</strong>layer_copy = Layer()
layer_copy.from_gltf("layer.gltf")</code></pre>

## Layer Modifiers

`Layer` offers a few handy methods which can modify models:

### Height Mapping

If you have 2D point data in one `Layer` and a mesh with the height information (such as terrain) in a different one, you can easily map the original 2D data onto the terrain:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
from metacity.io import parse_recursively

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))

trees = Layer()
trees.add_models(parse_recursively("trees"))

<strong>#Layer.map_to_height(self, layer: Layer) -> None
</strong>trees.map_to_height(terrain)</code></pre>

### Simplification

In case you need to simplify your geometry by approximating it with its crude envelope (not a perfect convex hull), you can use the following method:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
from metacity.io import parse_recursively

buildings = Layer()
buildings.add_models(parse_recursively("buildings"))

<strong>#Layer.simplify_envelope(self) -> None
</strong>buildings.simplify_envelope()</code></pre>

### Height Map Re-mesh

It is possible to re-mesh a model using a height-map approach. A grid of vertices is generated and placed "on top" of the source model, effectively covering it.&#x20;

The new mesh is divided into several tiles (each is a separate `Model`), and each tile is further divided according to the supplied parameters:

![In the example, the tile\_side is an arbitrary number (let's say 100), and tile\_divisions is equal to 4. The first tile is always aligned with the minimum coordinates of the Layer bounding box. The dotted lines correspond to edges in the newly generated mesh, bold dots are new vertices.](../.gitbook/assets/remesh.png)

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
from metacity.io import parse_recursively

terrain = Layer()
terrain.add_models(parse_recursively("terrain"))

<strong>#Layer.simplify_remesh_height(self, tile_side: float, tile_divisions: int) -> None
</strong>terrain.simplify_remesh_height(100, 4)</code></pre>
