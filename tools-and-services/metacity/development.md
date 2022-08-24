---
description: Developer notes regarding Metacity package
---

# Development

After uploading any code to the [Metacity GitHub repo](https://github.com/MetacitySuite/Metacity), the actions specified in [`test.yaml`](https://github.com/MetacitySuite/Metacity/blob/main/.github/workflows/test.yaml) are executed. Generally, it involves&#x20;

1. Building the C++ parts of the code
2. Running tests with `pytest` - see [Testing](development.md#tests)

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

There is a `release.sh` script that will let you update the version number and publish the updated package. Before you publish a new version to [pipy.org](https://pipy.org):

* **make sure the version you want to publish is in the `main` branch**
* you will need a password or a secret token (which can only be provided by [the cat](https://github.com/vojtatom))
* you will need a right to write into the `main` branch

The contents of the package are specified in `MANIFEST.in` located in the root of the repository.

The `release.sh` goes through the following steps:

1. version number is updated locally&#x20;
2. the update is pushed to the main branch
3. the latest commit is tagged with a version number
4. tags are pushed, [triggering a release ](https://github.com/MetacitySuite/Metacity/blob/main/.github/workflows/release.yaml)
5. the package is published on [pipy.org](https://pipy.org)

### Branches

| Branch  |                                                    Status                                                   | Description                             |
| ------- | :---------------------------------------------------------------------------------------------------------: | --------------------------------------- |
| release | ![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=release) | for production                          |
| dev     |   ![Build Status](https://github.com/MetacitySuite/Metacity/workflows/Metacity%20CI/badge.svg?branch=dev)   | merged PRs auto tested, for development |

