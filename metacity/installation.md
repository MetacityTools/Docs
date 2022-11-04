---
description: Metacity package installation
---

# Installation

You can install `Metacity` with [pip](https://pypi.org/project/metacity/):

```bash
pip install metacity
```

We support Python 3.8 to 3.10 and provide precompiled wheels for CPython on:

* Windows 64bit
* macOS 10.9+ (both Intel and Apple Silicon)
* `manylinux` and `musllinux` distros: ALT Linux 10+, RHEL 9+, Debian 11+, Fedora 34+, Mageia 8+, Photon OS 3.0 with updates, Ubuntu 21.04+, Apline 3.14+, and several others on 64 bit processors only

{% hint style="success" %}
Since version `0.5.2`, we provide precompiled `wheels`, so you don't have to compile anything locally. The build system relies on [`scikit-build`](https://github.com/scikit-build/scikit-build), [`pybind11`](https://github.com/pybind/pybind11), and [`cibuildwheel`](https://github.com/pypa/cibuildwheel). We do this to simplify the installation process as much as possible.&#x20;
{% endhint %}

If we don't provide `wheels` for your platform, you want to build an older version, or if you want to tweak the package code yourself, you can also clone the Metacity repo and [compile it using this guide](../tools-and-services/metacity/development.md#local-compilation).
