---
description: Python package geo data processing
---

# 🌆 Metacity

The Python package`metacity` acts as the entry data gateway. It consists of several sub-packages:

* `metacity.geometry` - code related to geometry processing
* `metacity.utils` - utilities for managing file systems and inspecting data
* `metacity.io` - importing and exporting data

### Installing Dependencies

This repository relies on system packages `GDAL` and `CMake`. Please make sure they are installed before trying to install the `Metacity` package.

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

### Installation

Requirements:

* If you don't want to edit the `Metacity` code,
* and if you have [`GDAL`](https://gdal.org) and [`CMake`](https://cmake.org) installed on your system (section [dependencies](metacity.md#undefined)),

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

## Structure

soon

