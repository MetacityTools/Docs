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

<table><thead><tr><th width="138.33333333333331">Branch</th><th width="219" align="center">Status</th><th>Description</th></tr></thead><tbody><tr><td>release</td><td align="center"><img src="https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=release" alt="Build Status"></td><td>for production</td></tr><tr><td>dev</td><td align="center"><img src="https://github.com/MetacitySuite/BananaGL/workflows/BananaGL%20CI/badge.svg?branch=dev" alt="Build Status"></td><td>for merged PRs, development branch</td></tr></tbody></table>

