---
description: Development notes regarding MetacityGL package
---

# Development notes

After uploading any code to the MetacityGL Repo, the actions specified in ci.yaml are executed. Generally, it involves building the library.

### Release

Everything merged into the branch `release` gets released. Before you publish a new version:

* **make sure the version number in your code is set up correctly**

### Branches

<table><thead><tr><th width="138.33333333333331">Branch</th><th width="219" align="center">Status</th><th>Description</th></tr></thead><tbody><tr><td>release</td><td align="center"><img src="https://github.com/MetacitySuite/MetacityGL/workflows/MetacityGL%20CI/badge.svg?branch=release" alt="Build Status"></td><td>for production</td></tr><tr><td>dev</td><td align="center"><img src="https://github.com/MetacitySuite/MetacityGL/workflows/MetacityGL%20CI/badge.svg?branch=dev" alt="Build Status"></td><td>for merged PRs, development branch</td></tr></tbody></table>

