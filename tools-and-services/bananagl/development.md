---
description: Developer notes regarding BananaGL library
---

# Development

After uploading any code to the [BananaGL Repo](https://github.com/MetacitySuite/BananaGL), the actions specified in [build.yml](https://github.com/MetacitySuite/BananaGL/blob/main/.github/workflows/build.yaml) are executed. Generally, it involves building the library.

### Release

There is a `release.sh` script that will let you update the version number and publish the updated package. Before you publish a new version:

* **make sure the version you want to publish is in the `main` branch**
* you will need a right to write into the `main` branch

The `release.sh` goes through the following steps:

1. version number is updated locally&#x20;
2. the update is pushed to the main branch
3. the latest commit is tagged with a version number
4. tags are pushed, [triggering a release ](https://github.com/MetacitySuite/BananaGL/blob/main/.github/workflows/release.yaml)

### Branches

| Branch |                                                                                              Status                                                                                             | Description                        |
| ------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------- |
| main   |                                             ![Build Status](https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=main)                                            | for production                     |
| dev    | [![Build Status](https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=dev)](https://github.com/MetacitySuite/Metacity/actions?query=workflow%3A%22Metacity+CI%22) | for merged PRs, development branch |

