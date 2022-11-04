---
description: 3D visualization library tailored for dynamic and urban data
---

# MetacityGL

MetacityGL is a React/Three.js library providing 3D visualization capabilities for React-based projects, specially tailored for dynamic and urban data vis.

{% hint style="info" %}
This library is a successor of [BananaGL](https://github.com/MetacityTools/BananaGL) and is currently under development.
{% endhint %}

## Usage

The library is separated into several modules. It is totally up to you how you use them; despite being react-based, utilizing some of the modules in react-free apps is also possible.

### Main Modules

The complete list (first-level only) of importable stuff follows:

```typescript
import { MetacityGL } from 'metacitygl'; //component
import { Graphics } from 'metacitygl';   //module
import { Utils } from 'metacitygl';      //module
import { Extensions } from 'metacitygl'; //module
import { Grid } from 'metacitygl';       //component
import 'metacitygl/style.css';           //styles for react components
```

* `MetacityGL` is a base react application component
* Module `Graphics` contains all rendering-related code
* Module `Utils` contains utilities that can help with parsing the input data
* Module `Extensions` contains all application or format-specific code (e.g. Metacity-react components)

