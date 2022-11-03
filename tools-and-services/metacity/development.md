---
description: Developer notes regarding Metacity package
---

# Development

After uploading any code to the Metacity GitHub repo, the actions specified in `ci.yaml` are executed. Generally, it involves&#x20;

1. Building the C++ parts of the code
2. Running tests with `pytest` - see [Testing](development.md#tests)

### Local compilation

Metacity is written in Python and C++ using [`pybind11`](https://github.com/pybind/pybind11). If you want to edit the package code yourself, we recommend following these steps first:&#x20;

Clone the repository:

```bash
git clone git@github.com:MetacitySuite/Metacity.git
```

(Optional but **strongly** recommended) Initialize local Python virtual environment

```bash
cd Metacity
python -m venv env
. ./env/bin/activate
```

Any time you make change to the **C++ or Python** code, you can build it with:

```bash
pip install .
```

{% hint style="info" %}
Up to version `0.5.3` there was a bug in the setup, which caused pollution of the python environment folder with files unrelated to the installation. This has been fixed.
{% endhint %}

### Debugging

If you encounter any problems, ensure you:

* have `CMake` installed
* have a C++ compiler supporting C++14 installed

### Testing

`Metacity` comes with a few testing datasets and uses [`pytest`](https://docs.pytest.org/en/7.1.x/). To run the tests locally, with coverage analysis, run:

```bash
python -m pytest tests/* --cov=metacity --cov-report term-missing
```

If you need to see the output of the tested code, run the command above with an extra flag `-s`

The tests are located inside the `tests` directory:

* `tests/conftest.py` contains [fixtures](https://docs.pytest.org/en/6.2.x/fixture.html) providing the testing data.
* `tests/test_module.py` contains individual tests, the `module` placeholder in the filename is usually replaced with the name of the tested Python module (not required)

Always test your code!

### Release

Everything merged into the branch `release` gets released. Before you publish a new version:

* **make sure the version number in your code is set up correctly**

The `version.sh` can inform you about the latest released versions.

### Branches

| Branch  |                                                    Status                                                   | Description                             |
| ------- | :---------------------------------------------------------------------------------------------------------: | --------------------------------------- |
| release | ![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=release) | for production                          |
| dev     |   ![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=dev)   | merged PRs auto tested, for development |

