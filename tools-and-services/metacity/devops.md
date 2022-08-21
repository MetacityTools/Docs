---
description: Developer notes regarding Metacity package
---

# DevOps

After uploading any code to the [Metacity GitHub repo](https://github.com/MetacitySuite/Metacity), the actions specified in [`ci.yml`](https://github.com/MetacitySuite/Metacity/blob/main/.github/workflows/ci.yml) are executed. Generally, it involves&#x20;

1. Building the C++ parts of the code
2. Running tests with `pytest` - see [Testing](devops.md#tests)

Additional actions can be triggered using the [Pull Request Flags](devops.md#pull-request-flags).

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

### Publishing

There is a `release.sh` script that will let you update the version number and publish the updated package. Before you publish a new version to [pipy.org](https://pipy.org):

* you will need a password or a secret token (which can only be provided by [the cat](https://github.com/vojtatom))

To publish the code from your local computer, run the following commands:

```
rm -rf dist;
rm -rf metacity.egg*;
python setup.py sdist; 
python -m twine upload dist/*;
```

The contents of the package are specified in `MANIFEST.in` located in the root of the repository.

### Branches

| Branch | Status                                                                                                                                                                                           | Description                       |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------- |
| main   | [![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=main)](https://github.com/MetacitySuite/Metacity/actions?query=workflow%3A%22Metacity+CI%22) | protected, merged PRs auto tested |
| dev    | [![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=dev)](https://github.com/MetacitySuite/Metacity/actions?query=workflow%3A%22Metacity+CI%22)  | merged PRs auto tested            |

