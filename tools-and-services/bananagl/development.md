---
description: Developer notes regarding BananaGL library
---

# Development

After uploading any code to the BananaGL Repo, the actions specified in build.yml are executed. Generally, it involves building the library.

### Release

Everything merged into the branch `release` gets released. Before you publish a new version:

* **make sure the version number in your code is set up correctly**

The `version.sh` can inform you about the latest released versions.

### Branches

| Branch  |                                                    Status                                                   | Description                        |
| ------- | :---------------------------------------------------------------------------------------------------------: | ---------------------------------- |
| release | ![Build Status](https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=release) | for production                     |
| dev     |   ![Build Status](https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=dev)   | for merged PRs, development branch |

