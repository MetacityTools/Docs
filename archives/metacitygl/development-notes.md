---
description: Development notes regarding MetacityGL package
---

# Development notes

After uploading any code to the MetacityGL Repo, the actions specified in ci.yaml are executed. Generally, it involves building the library.

### Release

Everything merged into the branch `release` gets released. Before you publish a new version:

* **make sure the version number in your code is set up correctly**

### Branches

| Branch  |                                                      Status                                                     | Description                        |
| ------- | :-------------------------------------------------------------------------------------------------------------: | ---------------------------------- |
| release | ![Build Status](https://github.com/MetacitySuite/MetacityGL/workflows/MetacityGL%20CI/badge.svg?branch=release) | for production                     |
| dev     |   ![Build Status](https://github.com/MetacitySuite/MetacityGL/workflows/MetacityGL%20CI/badge.svg?branch=dev)   | for merged PRs, development branch |

