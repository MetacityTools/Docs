---
description: Python package for Geodata processing
---

# 🌆 Metacity

The `Metacity` package allows to preprocess geospatial data and export it in a more suitable form for web visualization.&#x20;

### Installation

The package relies on system packages [`GDAL`](https://gdal.org) and [`CMake`](https://cmake.org). Please make sure they are installed before trying to install the `Metacity` package.

1. Install [GDAL](https://mothergeo-py.readthedocs.io/en/latest/development/how-to/gdal-ubuntu-pkg.html)

```bash
sudo add-apt-repository ppa:ubuntugis/ppa
sudo apt-get update
sudo apt-get install gdal-bin
```

1. Install [CMake](https://cmake.org/download/)

```bash
sudo apt-get install cmake
```

* If you don't want to edit the `Metacity` code,
* and if you have [`GDAL`](https://gdal.org) and [`CMake`](https://cmake.org) installed on your system,

you can install `Metacity` with [pip](https://pypi.org/project/metacity/):

```bash
pip install metacity
```

### How to compile locally

Metacity is written in Python and C++ using [`pybind11`](https://github.com/pybind/pybind11). If you want to edit the package code yourself, we recommend following these steps first:&#x20;

Clone the repository:

```bash
git clone git@github.com:MetacitySuite/Metacity.git
```

(Optional) Initialize local Python virtual environment

```bash
cd Metacity
python -m venv env
. ./env/bin/activate
```

Inside `Metacity` directory initialize `pybind11` submodule:

```bash
git submodule update --init --recursive
```

Install requirements:

```bash
pip install -r requirements.txt
```

Any time you make change to the C++ code, you can build it with:

```bash
python setup.py build_ext --inplace
```

### Tests

`Metacity` comes with a few testing datasets and uses [`pytest`](https://docs.pytest.org/en/7.1.x/) for testing. To run the tests locally, with coverage analysis, run:

```bash
python -m pytest tests/* --cov=metacity --cov-report term-missing
```

If you need to see the output of the tested code, run the command above with an extra flag `-s`

The tests are located inside `tests`directory:

* `tests/conftest.py` contains [fixtures](https://docs.pytest.org/en/6.2.x/fixture.html) providing the testing data.
* `tests/test_module.py` contains individual tests, the `module` placeholder in the filename is usually replaced with the name of the tested Python module (not required)

Always test your code!

### Publishing

Before you publish a new version:

* you will need a password or a secret token (which can only be provided by [the cat](https://github.com/vojtatom))
* make sure the version number in `setup.py` is correct and not behind [the last published version](https://pypi.org/project/metacity/)

To publish the code, run the following commands:

```
rm -rf dist;
rm -rf metacity.egg*;
python setup.py sdist; 
python -m twine upload dist/*;
```

What is going to end up inside the published package is specified in `MANIFEST.in` that is located in the root of the repository.

## Usage

The Python package`metacity` acts as the entry data gateway. It consists of several sub-packages:

* `metacity.io` - importing and exporting data
* `metacity.geometry` - geometry processing
* `metacity.utils` - managing file systems and inspecting data

The easiest way to understand `Metacity` is to think of it as a _pipeline_.

### Data Import

Metacity currently supports the following formats:

| Format    | Suffix  | Reference                                                                                                                                             |
| --------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Shapefile | `.shp`  | [ESRI Shapefile Technical Description](https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf) |
| GeoJSON   | `.json` | [The GeoJSON Specification (RFC 7946)](https://geojson.org)                                                                                           |

Importing data is fairly easy with the functionalities provided by the `metacity.io` module:

#### Importing a single file

```python
from metacity.io import parse
models = parse("data/file.shp", from_crs="EPSG:4326", to_crs="EPSG:5514")
```

The `parse` function loads contents of a provided file and optionally transforms it from one coordinate reference system to another. It returns a list of `Models`.

#### Importing multiple files

Often, the geospatial data is partitioned into several files and scattered among several directories. If you wish to import all of the data located in the directory tree under a certain folder, you can do:

```python
from metacity.io import parse_recursively
models = parse_recursively("data", from_crs="EPSG:4326", to_crs="EPSG:5514")
```

The returned value is a flattened list of `Models` regardless of how many files were processed.

### Models = Metadata + Attributes

A `Model` is a universal entity for storing geometry and metadata, it has no specific semantic meaning.&#x20;

* The geometry is stored in `Attributes`. Models can have multiple `Attributes`, see section [Attributes](metacity.md#attributes).
* The properties can be attached to a model as a metadata

See the following example:

```python
from metacity.geometry import Model, Attribute

model = Model()
position_attr = Attribute()
position_attr.push_polygon3D([[0, 0, 0, \
                               0, 0, 1, \
                               0, 1, 1]])
                          
#Model.add_attribute(self, arg0: str, arg1: Attribute) -> None
model.add_attribute("POSITION", position_attr)

#Model.set_metadata(self, arg0: dict) -> None
model.set_metadata({ "description": "An example triangle" })
```

Additionally, the `Model` class has the following methods:

```python
#Complementary to method add_attribute, there is also 
#Model.attribute_exists(self, arg0: str) -> bool
position_exists = model.attribute_exists("POSITION")

#Model.get_attribute(self, arg0: str) -> Attribute
position_attr = model.get_attribute("POSITION")

#@property
#Model.metadata(self) -> dict
metadata = model.metadata
```

Note that the property `Model.metadata` returns a copy. Updating it won't affect the metadata stored inside the model. If you wish to update the metadata, use the `Model.set_metadata` method:

```python
#updating the metadata
#Model.set_metadata(self, arg0: dict) -> None
model.set_metadata({ "description": "A new description" })
```

#### Attributes&#x20;

`Attributes` work similarly to [GLTF buffers](https://www.khronos.org/files/gltf20-reference-guide.pdf). The `Attribute` API allows parsing of various data. All data is stored inside as if it was 3D data (2D data gets padded by zeroes).&#x20;

A unique name identifies each attribute. There are a few names that are commonly used for certain types of attributes:

| Name       | Description                         |
| ---------- | ----------------------------------- |
| `POSITION` | coordinates of the vertex positions |
| `NORMAL`   | normal buffer                       |
| `COLOR`    | vertex color                        |

`Metacity` does not utilize indices for geometry reuse, all data is stored "as is" in sequential order.

If everything works as intended, a user **should rarely work directly with** `Attributes`. Occasionally, it might be useful to be able to access the Attribute API:

```python
from metacity.geometry import Attribute

points = Attribute()
#Insert 2D points using push_line2D method:
#Attribute.push_point2D(self, arg0: List[float]) -> None
points.push_line2D([0, 0, \
                    1, 1, \
                    1, 2])
#Similar for 3D points
#Attribute.push_point3D(self, arg0: List[float]) -> None
points.push_point3D([0, 0, 0,   \
                     1, 1, 0.5, \
                     1, 2, 1])
```

Parsing Lines is very similar to parsing points, although the main difference is the data gets stored as individual segments duplicating inner vertices.  &#x20;

```python
from metacity.geometry import Attribute

lines = Attribute()
#Insert 2D line string using push_line2D method:
#Attribute.push_line2D(self, arg0: List[float]) -> None
lines.push_line2D([0, 0, \
                    1, 1, \
                    1, 2])
#in the example above, the vertices get internally stored as:
#input:          a - b - c
#stored values: [0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 2, 0]
#               [a - b, b - c]
#               every 3rd zero is padding for 3D data, inner vertex si duplicated 

#Similar for 3D points
#Attribute.push_line3D(self, arg0: List[float]) -> None
lines.push_line3D([0, 0, 0,   \
                    1, 1, 0.5, \
                    1, 2, 1])
```

Polygons are automatically triangulated, the API also supports polygons with holes:

```python
from metacity.geometry import Attribute

points = Attribute()
#Insert simple 2D polygon using push_polygon3D method:
#Attribute.push_polygon2D(self, arg0: List[List[float]]) -> None
position.push_polygon2D([[0, 0, \
                          0, 1, \
                          1, 1, \
                          0, 1]])

#Insert 2D polygon with hole in the middle:
position.push_polygon2D([[0, 0, \
                          0, 1, \
                          1, 1, \
                          0, 1], \
                          [0.25, 0.25, \
                           0.25, 0.75,
                           0.75, 0.75,
                           0.75, 0.25])
#The structure draws from GeoJSON specs - 
#the List[List[float]] can be interpreted as
#[[polygon], [hole], [hole] ...] 

#All works equivalently for 3D:
#Attribute.push_polygon3D(self, arg0: List[List[float]]) -> None    
position.push_polygon3D([[0, 0, 1, \
                          0, 1, 1, \
                          1, 1, 1, \
                          0, 1, 1]])                       
```

### Layers

Sometimes, it is handy to organize things into groups. In Metacity, you can use `Layers`.

```python
from metacity.geometry import Layer, Model
layer = Layer()

models = [Model(), Model()]
#Layer.add_models(models: List[Model]) -> None
layer.add_models(models)
```

It is also possible to add a single model:

```python
from metacity.geometry import Layer, Model
layer = Layer()

model = Model()
#Layer.add_model(model: Model) -> None
layer.add_models(model)
```

You can access the individual models again. Deleting a `Model` from the returned list does not remove it from the `Layer`.  The models are not copied, the returned list contains references to the models stored inside Layer.

```python
from metacity.geometry import Layer
layer = Layer()
#... loading data, processing it

#Layer.get_models() -> List[Model]
models = layer.get_models()
```

Moreover, it is possible to store and load the contents of a `Layer` to and from the `.gltf` format.&#x20;

<pre class="language-python"><code class="lang-python">from metacity.geometry import Layer
layer = Layer()
#... loading data, processing it

#Store the data
#Layer.to_gltf(filename: str) -> None
layer.to_gltf("layer.gltf")

#Load the data
<strong>#Layer.from_gltf(filename: str) -> None
</strong><strong>layer_copy = Layer()
</strong>layer_copy.from_gltf("layer.gltf")</code></pre>
