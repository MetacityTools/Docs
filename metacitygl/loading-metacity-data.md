---
description: How to display data processed by MetacityGL
---

# Loading Metacity Data

MetacityGL contains layers specifically designed for displaying data processed by the Metacity package - `Extensions.MetacityLayer` and `Extensions.MetacityTreeLayer`.

## Grid Layer

<pre class="language-tsx"><code class="lang-tsx">import React from 'react'
import { MetacityGL, Extensions } from 'metacitygl';
import 'metacitygl/style.css';

<strong>export function Application() {
</strong>
    React.useEffect(() => {
        return () => {
            //recommended force cleaning up the rendering context
            window.location.reload();
        }
    }, []);

    return (
        &#x3C;MetacityGL background={0x000000}>
            &#x3C;Extensions.MetacityLayer
                api="https://api.some.source/dataset"
                color={0x00728a}
            >
                &#x3C;div>Layer UI&#x3C;/div>
            &#x3C;/Extensions.MetacityLaye>
        &#x3C;/MetacityGL>
    )
}</code></pre>

The component `Extensions.MetacityLayer` is designed for parsing and displaying data exported from [Grids](../metacity/grids.md). The layer props look like this:&#x20;

```jsx
interface LayerProps extends MetacityGL.MetacityLayerProps {
    api: string;
    pickable?: boolean;
    color?: number;
    colorPlaceholder?: number;
    styles?: MetacityGL.Utils.Styles.Style[];
    radius?: number;
    instance?: string;
    size?: number;
    swapDistance?: number;
    skipObjects?: number[];
    enableUI?: boolean;
    
    //DO NOT SET - set by the MetacityGL parent tag:
    children?: React.ReactNode;
    context?: GraphicsContext;
    onLoaded?: CallableFunction;
}
```

| prop               | description                                                            |
| ------------------ | ---------------------------------------------------------------------- |
| `api`              | data source api                                                        |
| `pickable`         | display metadata on hover - enable only one layer in the visualization |
| `color`            | hex represtation of the layer base color                               |
| `colorPlaceholder` | color of unloaded tile placeholder                                     |
| `styles`           | see [Styles](color-and-styles.md)                                      |
| `radius`           | minimal distance for tile loading                                      |
| `instance`         | `url` of GLTF model to be displyed for point data                      |
| `size`             | size of the point models                                               |
| `swapDistance`     | distance behind which point models are replaced by rectangles          |
| `skipObjects`      | list of object IDs that stay hidden                                    |
| `enableUI`         | flag that signals the layer UI should get rendered                     |

## Tree Layer

