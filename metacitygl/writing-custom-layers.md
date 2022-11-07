---
description: How to display any general data
---

# Writing Custom Layers

Each layer's props should extend the `MetacityLayerProps` interface:

```tsx
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

The returned nodes will get automatically attached to the Metacity UI panel. You can make the UI look however you want.&#x20;

Each layer gets these props by default:

* `context` is a class encapsulating the graphics context, you can use it to modify what gets rendered
* `onLoaded` is a callback you must use to signal the application that your layer is ready to display
* `enableUI` is a flag that signals you whether the layer UI should get displayed or not

## Parsing data

There is a recommended way how to handle any data in MetacityGL. In your custom layer, split the process into 3 parts:

```tsx
//... other imports
import axios from "axios";
import { Utils } from "metacitygl"

export function CustomLayer(props: MetacityLayerProps) {
    const { context, onLoaded, enableUI } = props;
    
    React.useEffect(() => {
         if (!context)
             return; 
             
         //get your data
         const data = (await axios.get(api)).data as Data;
         
         //parse it using Metacity's Assemblers
         const asm = new Utils.Assemblers.SomeAssembler();
         for (const item in data) {
             //pass the item to the assembler
         }
         if (asm.empty)
             return;
         
         const buffers = asm.toBuffers();
         //transform it into a Model
         const model = Graphics.Models.SomeModel.create(buffers);
         //pass it to context
         context.add(model);
        
    }, [context]);
    
    return (...)
}
```

1. Getting your data is probably the easiest part. Use a library you like (e.g. [`axios`](https://axios-http.com/docs/intro)?) and download it.
2. Transforming the data into a renderable model can be tricky. MetacityGL provides built-in tools called [Assemblers](assemblers.md) that can make your life a little easier. The Assemblers allow you to parse the data incrementally. When you are done, you can export the data from the Assembler in a form that can be directly passed into one of Metacity's Models.
3. Create a [Model](models-and-materials.md) - you can create a completely new one using [three.js](https://threejs.org) or reuse one defined in the library.
4. Pass the created model into the graphics context.&#x20;

### Workers

The data-parsing steps usually require some number-crunching. It is best to isolate these steps into a separate worker process and pass the result back to the layer component. &#x20;

{% code title="dataWorker.ts" %}
```tsx
import axios from "axios";
import { Utils } from "metacitygl"

declare var self: any;

interface Data {
    //TODO
}

self.onmessage = async function (e: any) {
    const { api, ... } = e.data;
    const data = (await axios.get(api)).data as Data;

    const asm = new Utils.Assemblers.SomeAssembler();
    for (const item in data) {
        //pass the item to the assembler
    }
    
    if (!asm.empty) {
        const buffers = asm.toBuffers();
        const transferables = asm.pickTransferables(buffers);
        self.postMessage(buffers, transferables);
    }
}
```
{% endcode %}

With this worker, you can structure the layer the following way:

```tsx
import Worker from "./dataWorker?worker&inline";
import { Graphics } from "metacitygl"

export function CustomLayer(props: MetacityLayerProps) {
    const { context, onLoaded, enableUI } = props;
    
    React.useEffect(() => {
        if (!context)
             return; 
        
        // instantiate a worker     
        const worker = new Worker();
        
        //start a job
        worker.postMessage({
           api: "https://my.api"
        });
        
        //process the job result
        worker.onmessage = (e) => {
            worker.terminate();
            const model = Graphics.Models.SomeModel.create(e.data);
            context.add(model);
        };  
    }, [context]);
    
    return (...)
}
```

There is much more you could do:&#x20;

* store the reference to the Model in the layer and allow modifying it with the UI
* you could create a pool of workers and reuse them between layers instead of creating and terminating them for better performance, etc.&#x20;

See the other pages for a better grasp of [Models](models-and-materials.md), [Assemblers](assemblers.md), and the [context](graphic-context.md).&#x20;
