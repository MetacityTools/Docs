---
description: Python package for Geodata processing
---

# 🌆 Metacity

_What is this good for?_

The `Metacity` package allows us to preprocess geospatial data and export it in a form that will be more suitable for web visualization.&#x20;

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

### Models

TODO

### Layers

Sometimes, it is handy to organize things into groups. In Metacity, you can use `Layers`.

```python
from metacity.geometry import Layer
layer = Layer()
layer.add_models(models)
```

#### Methods

**`Layer.add_model(model: Model) -> None`**\
``Adds a single model into the layer, returns `None`

**`Layer.add_models(models: List[Model]) -> None`**\
``Adds a list of models into the layer, returns `None`

**`Layer.get_models() -> List[Model]`**\
``Returns a list of `Models` stored in `Layer`. Deleting a `Model` from the returned list does not remove it from the `Layer`. The models are not copied.

**`Layer.from_gltf(filename: str) -> None`**\
``Loads a `Layer` from `.gltf` file. All geometry and metadata are preserved.

**`Layer.to_gltf(filename: str) -> None`**\
``Saves `Layer` contents into a `.gltf` file. All geometry and metadata are preserved.

### Grid
