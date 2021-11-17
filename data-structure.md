---
description: This page describes the internal Metacity Data Model.
---

# 🧬 Data Model

## Basic Model Hierarchy

The Metacity Data Structure is designed to allow for easy data manipulation and streaming. The structure contains a several data sub-models, which are organized into a hierarchy described bellow. To avoid confusion, we refer to the global data model as the data structure.&#x20;

| Title                                                                       | Description                                                                                                                                                                                                                                                                   |
| --------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>Primitive Model</code><br><code>(Primitive)</code></p>             | <p>The primary geometrical element, has LoD value assigned from range 0 to 4</p><p>Supported geometrical types are <em>points</em>, <em>lines</em> and <em>faces</em></p>                                                                                                     |
| `Object`                                                                    | <p>The primary structural element with metadata attributes </p><p>Encapsulates a set of <em>Primitives</em></p><p>The encapsulated <em>Primitives</em> can be of a different type; however, each type of <em>Primitive</em> with a specific LoD can be present only once </p> |
| <p><code>Regular Grid Tile Model</code></p><p><code>(Tile Model)</code></p> | <p>A compilation of <em>Primitives</em> of a certian type (<em>points</em>, <em>lines</em> or <em>faces</em>)</p><p>Has LoD value assigned from range 0 to 4</p><p>Contains links to the original <em>Primitives</em></p>                                                     |
| <p><code>Regular Grid Tile</code></p><p><code>(Tile Object)</code></p>      | <p>A tile with a limited axis-aligned bounding box<br>Encapsulates a set of <em>Regular Grid Tile Models</em></p><p>Contains slices of <em>Objects</em> contained in the tile bounding box </p>                                                                               |
| `Regular Grid`                                                              | A set of _Regular Grid Tiles_                                                                                                                                                                                                                                                 |
| `Layer`                                                                     | A set of _Objects_ and their representation organized into a _Regular Grid_                                                                                                                                                                                                   |

The structure is based on the following principles:

* A unique id identifies each _Object_
* The entire structure is stored in a file system as a structure of directories and files
* _Layer_ also contains data required for the assembly and modification of a_ Regular Grid_

## Project Structure

The simplified project structure with a single _Layer_ can be represented as following directory tree:

```
project/
└── layer
    ├── config.json
    ├── geometry
    │   └── Geometry Layout
    │   ...
    │    
    ├── grid
    │   ├── grid.json
    │   ├── cache
    │   │   └── Cache Layout
    │   │   ...
    │   │
    │   └── tiles
    │       └── Tile Layout
    │       ...
    │
    └── metadata
        └── Object Metadata
        ...
```

The structure consists of the following subparts:

| File/Folder      | Description                                                                                 | Renamable? |
| ---------------- | ------------------------------------------------------------------------------------------- | ---------- |
| `project`        | The main project directory                                                                  | ✅          |
| `layer`          | The layer directory                                                                         | ✅          |
| `config.json`    | Layer config file                                                                           | ❌          |
| `geometry`       | Contains original _Object_ files                                                            | ❌          |
| `grid`           | Contains _Regular Grid_ data                                                                | ❌          |
| `grid/grid.json` | Grid config file                                                                            | ❌          |
| `grid/cache`     | Directory containing _Object_ files sliced according to _Regular Grid Tile_ bounding boxes. | ❌          |
| `grid/tiles`     | Directory containing _Regular Grid Tiles _assembled from _Objects_ in `grid/cache`          | ❌          |
| `metadata`       | Directory containing _Object_ metadata                                                      | ❌          |

There are 4 directory sublayouts in which the data is organized: `Geometry Layout`, `Cache Layout`, `Tile Layout`, and `Object Metadata `.

### Geometry Layout

`Geometry Layout` consists of the following directory structure:

```
├── facets
│   ├── 0
│   │   ├── ObjectID1.json      ┐
│   │   ...                     ├── Primitive Models 
│   │   └── ObjectID456.json    ┘   with type Facet, LoD 0
│   ├── 1
│   ├── 2
│   ├── 3
│   └── 4
├── lines
│   ├── 0
│   ...
│   └── 4
└── points
    ├── 0
    ...
    └── 4
```

Each of the directories 0 to 4 contains _Primitives_ of the type specified by its parent directory. The name of the files corresponds to the ID of the _Object_ the _Primitive_ is assigned to. Therefore, a single _Object_ cannot encapsulate two _Primitives_ of the same type and LoD.

### Cache Layout

The structure of the `Cache Layout` is actually variable, but can be represented by the following directory tree:

```
For each existing tile with indices x, y:
├── x_y
│   └── Geometry Layout
│   ...
...
```

The `Geometry Layout` inside the x\_y tile directory contains only the objects, or their slices, which are contained in the _xy_ tile.

### Tile Layout

The structure is simmilar to `Cache Layout`, although there are minor differences. The original `Geometry Layout` is inverted and simplified:

```
For each existing tile with indices x, y:
├── x_y
│   ├── 0
│   │   ├── facets.json    ┐
│   │   ├── lines.json     ├── Regular Grid Tile Models
│   │   └── points.json    ┘
    ...
│   └── 4
│   ...
...
```

### Object Metadata

Object Metadata Layout is a simple list of json files. The files are linked to the corresponding _Object_ by its filename:&#x20;

```
For each existing object with ObjectID:
├── ObjectID.json
```

## Models

TODO

## Example Project Structure

Individual layers create a project. Here is a complete structure of a project with single layer and 42 facet objects with LoD 1 and 2.&#x20;

```
project/
└── layer1
    ├── config.json
    ├── geometry
    │   ├── facets
    │   │   ├── 0
    │   │   ├── 1
    │   │   │   ├── ObjectID1.json
    │   │   │   │   ...
    │   │   │   └── ObjectID42.json
    │   │   ├── 2
    │   │   │   ├── ObjectID1.json
    │   │   │   │   ...
    │   │   │   └── ObjectID42.json
    │   │   ├── 3
    │   │   └── 4
    │   ├── lines
    │   │    ...
    │   └── points
    │        ...
    ├── grid
    │   ├── grid.json
    │   ├── cache
    │   │   ├── 0_0
    │   │   │   ├── facets
    │   │   │   │   ├── 0
    │   │   │   │   ├── 1
    │   │   │   │   │   ├── ObjectID12.json
    │   │   │   │   │   │    ...
    │   │   │   │   │   └── ObjectID15.json
    │   │   │   │   ├── 2
    │   │   │   │   │   ├── ObjectID12.json
    │   │   │   │   │   │    ...
    │   │   │   │   │   └── ObjectID15.json
    │   │   │   │   ├── 3
    │   │   │   │   └── 4
    │   │   │   ├── lines
    │   │   │   │    ...
    │   │   │   └── points
    │   │   │        ...
    │   │   ├── 0_1
    │   │   ...
    │   │
    │   └── tiles
    │       ├── 0_0
    │       │   ├── 0
    │       │   ├── 1
    │       │   │   └── facets.json
    │       │   ├── 2
    │       │   │   └── facets.json
    │       │   ├── 3
    │       │   ├── 4
    │       │   └── config.json
    │       ├── 0_1
    │       ...
    │
    └── metadata
        ├── ObjectID1.json
        │   ...       
        └── ObjectID42.json

```





