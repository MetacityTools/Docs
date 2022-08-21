---
description: 3D Web Visualization Library
---

# 🍌 BananaGL

Visualize data preprocessed by Metacity with [BananaGL](https://github.com/MetacitySuite/BananaGL) - a client-side web visualization library.&#x20;

### Quick Start Guide

See [Releases](https://github.com/MetacitySuite/BananaGL/releases/) on our GitHub.&#x20;

1. Download [`bananagl.zip`](https://github.com/MetacitySuite/BananaGL/releases) (see Assets) and use its contents in your project.
2. Create `index.html` and start coding!

Minimal HTML starting template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BananaGL Project</title>
</head>
<body>
    <canvas id="canvas">Your browser does not support canvas</canvas>
    <script src="bananagl.js"></script>
    <script>
        window.onload = () => {
            const canvas = document.getElementById("canvas");

            const gl = new BananaGL.BananaGL({ 
                canvas: canvas,
                loaderPath: "loaderWorker.js",
            });
    </script>
</body>
</html>
```

## Initialization

First, you need to initialize the library:

```javascript
const gl = new BananaGL.BananaGL({ 
    canvas: canvas,
    loaderPath: "loaderWorker.js",
});
```

The function accepts a parameter object with the following properties:

| Parameter                      | Required | Description                                                                                                                                                          |
| ------------------------------ | :------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>canvas</code><br></p> |     ✅    | `HTMLCanvasElement`                                                                                                                                                  |
| `loaderPath`                   |     ✅    | `string`, a path to background Worker JS file which handles data loading                                                                                             |
| `background`                   |     -    | `number`, such as `0xffffff` defining the background color                                                                                                           |
| `far`                          |     -    | `number`, camera far                                                                                                                                                 |
| `maxDistance`                  |     -    | `number`, limits how far can user zoom out                                                                                                                           |
| `maxPolarAngle`                |     -    | `number`, limits how low can user rotate the camera, horizontal view corresponds to `Math.PI / 2`                                                                    |
| `minDistance`                  |     -    | `number`, limits how close can user zoom in                                                                                                                          |
| `minPolarAngle`                |     -    | `number`, limits how high can user rotate the camera, top view corresponds to `0`, but for numerical stability reasons it is advisable to use something like `0.001` |
| `near`                         |     -    | `number`, camera near                                                                                                                                                |
| `offset`                       |     -    | `number`, when the camera is initialized in _isometric_ style of view, this is the camera offset from the camera target (focus point)                                |
| `position`                     |     -    | `number`, initial position of the camera, overwrites the coordinates passed in URL                                                                                   |
| `target`                       |     -    | `number`, initial target of the camera, overwrites the coordinates passed in URL                                                                                     |
| `zoomSpeed`                    |     -    | `number`,                                                                                                                                                            |



## Layer

You can add new geographic layers to you visualization following way:

```javascript
const gl = new BananaGL.BananaGL({...});

gl.loadLayer({
    api: "https://data.metacity.cc/terrain",
    baseColor: 0xffffff
});
```

