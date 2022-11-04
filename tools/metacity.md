---
description: Python package for Geodata processing
---

# Metacity

The `Metacity` package allows to preprocess geospatial data and export it to a more suitable form for web visualization.&#x20;

## Usage

The Python package`metacity` acts as the entry data gateway. The easiest way to understand `Metacity` is to think of it as a _pipeline_. To prepare your data for visualization, you will need to:

1. [Import your data](../metacity/data-import.md)
2. [Create new Layer(s)](../metacity/layers.md)
3. Optionally optimize and modify the data (see [Layer Modifiers](../metacity/layers.md#layer-modifiers))
4. Convert Layers selected for visualization to [Grid](../metacity/grids.md) or [Quad-trees ](../metacity/quad-trees.md)
5. Optionally optimize the data (see [Grid Modifiers](../metacity/grids.md#grid-modifiers) or [Quad-tree Modifiers](../metacity/quad-trees.md#quad-tree-modifiers))
6. Export data for streaming from [Grids](../metacity/grids.md#exporting-data) or [Quad-trees](../metacity/quad-trees.md#exporting-data)

### Structure

`Metacity` consists of several sub-packages:

* `metacity.io` - importing and exporting data
* `metacity.geometry` - geometry processing
* `metacity.utils` - managing file systems and inspecting data
* `metacity.pipeline` - CLI tool for quick edits - deprecated



&#x20;&#x20;
