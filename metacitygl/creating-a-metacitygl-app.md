---
description: Making a react-based app with MetacityGL
---

# Creating a MetacityGL App

`MetacityGL` is developed with [vitejs](https://vitejs.dev), but you can use whichever react-based toolbox you want. This short tutorial will outline a few recommended ways to build a simple MetacityGL app. You can get your local setup running using[ this guide](https://vitejs.dev/guide/).

An empty application could look like this:

<pre class="language-typescript"><code class="lang-typescript">import React from 'react'
import { MetacityGL, Utils } from 'metacitygl';
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
            //insert your layers here
        &#x3C;/MetacityGL>
    )
}
</code></pre>

The `<MetacityGL>` tag takes care of graphics context, rendering canvas, navigation, etc., and passes a reference to the context to its children components. That means you can write layer components that can do anything and make things appear in the visualization using the provided API.&#x20;

Each layer should extend the `LayerProps` interface:

```typescript
import { MetacityLayerProps } from 'metacitygl';

export function CustomLayer(props: MetacityLayerProps) {
    const { context, onLoaded, enableUI } = props;
    return (
        <div>
            <h1>Custom Layer</h1>
            <p>This is a custom layer</p>
        </div>
    )
}
```

For more info on how to handle your data, see [Writing Custom Layer](writing-custom-layers.md)s. If you wish to serve content produced by Metacity, see [Loading Metacity Data](loading-metacity-data.md).

&#x20;
