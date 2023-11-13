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
            &#x3C;/Extensions.MetacityLayer>
        &#x3C;/MetacityGL>
    )
}
</code></pre>

The component `Extensions.MetacityLayer` is designed for parsing and displaying data exported from [Grids](../../metacity/grids.md). The layer props look like this:&#x20;

```jsx
interface LayerProps {
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

<table><thead><tr><th width="266">prop</th><th>description</th></tr></thead><tbody><tr><td><code>api</code></td><td>data source api</td></tr><tr><td><code>pickable</code></td><td>display metadata on hover - enable only one layer in the visualization</td></tr><tr><td><code>color</code></td><td>hex represtation of the layer base color</td></tr><tr><td><code>colorPlaceholder</code></td><td>color of unloaded tile placeholder</td></tr><tr><td><code>styles</code></td><td>see <a href="color-and-styles.md">Styles</a></td></tr><tr><td><code>radius</code></td><td>minimal distance for tile loading </td></tr><tr><td><code>instance</code></td><td><code>url</code> of GLTF model to be displyed for point data</td></tr><tr><td><code>size</code></td><td>size of the point models</td></tr><tr><td><code>swapDistance</code></td><td>distance behind which point models are replaced by rectangles </td></tr><tr><td><code>skipObjects</code></td><td>list of object IDs that stay hidden</td></tr><tr><td><code>enableUI</code></td><td>flag that signals the layer UI should get rendered</td></tr></tbody></table>

## Tree Layer

```tsx
import React from 'react'
import { MetacityGL, Extensions } from 'metacitygl';
import 'metacitygl/style.css';

export function Application() {

    React.useEffect(() => {
        return () => {
            //recommended force cleaning up the rendering context
            window.location.reload();
        }
    }, []);

    return (
        <MetacityGL background={0x000000}>
            <Extensions.MetacityTreeLayer
                api="https://api.some.source/dataset"
                color={0x00728a}
            >
                <div>Layer UI</div>
            </Extensions.MetacityTreeLayer>
        </MetacityGL>
    )
}
```

`Extensions.MetacityTreeLayer` extends the `Extensions.MetacityLayer` and allow rendering the Quad-tree built on top of the data. The layer props extend the props from the previous section:

```tsx
interface TreeLayerProps extends LayerProps {
    loadingRadius?: number;
    requestTileRadius?: number;
    distFactor?: number;
    distZFactor?: number;
    radFactor?: number;
    visualizeTree?: boolean;
    zOffset?: number;
}
```

<table><thead><tr><th width="238">prop</th><th>description</th></tr></thead><tbody><tr><td><code>loadingRadius</code></td><td>multiplicative factor influencing the loading distance of individual tree nodes</td></tr><tr><td><code>requestTileRadius</code></td><td>equivalent of <code>loadingRadius</code> but for loading the actual model</td></tr><tr><td><code>distFactor</code></td><td>exponent factor influencing the loading distance of individual tree nodes in the XY plane - used for computation of the camera distance</td></tr><tr><td><code>distZFactor</code></td><td>exponent factor influencing the loading distance of individual tree nodes in the Z direction - used for computation of the camera distance</td></tr><tr><td><code>radFactor</code></td><td>exponent factor influencing the loading distance of individual tree nodes - used for computation of the loading threshold</td></tr><tr><td><code>visualizeTree</code></td><td>flag whether to render the tree</td></tr><tr><td><code>zOffset</code></td><td>offsetting the tree model in the Z direction</td></tr></tbody></table>

