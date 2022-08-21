---
description: 3D Web Visualization Library
---

# 🍌 BananaGL

Visualize data preprocessed by Metacity with [BananaGL](https://github.com/MetacitySuite/BananaGL) - a client-side web visualization library. It is built with [three.js](https://threejs.org/) and `Typescript`.&#x20;

* 👩‍💻 Newest releases are always available [on our GitHub](https://github.com/MetacitySuite/BananaGL/releases)
* 📦 Datasets can be obtained from our [DataAPI](https://api.metacity.cc/) service

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
| `zoomSpeed`                    |     -    | `number`, speed of zooming                                                                                                                                           |

### How Camera works

Camera has two significant attributes which influence the LOD loading, etc.:

* `position` - actual position of the camera in 3D space
* `target` - focus point of the camera, always lies in the `z = 0` plane

Despite its looks, `BananaGL` uses Perspective camera with `fov = 5`, and by default all limits are setup for viewing data in coordinate system `EPSG:5514`.

## Layer

You can add new geographic layers to you visualization following way:

```javascript
const gl = new BananaGL.BananaGL({...});

gl.loadLayer({
    api: "https://data.metacity.cc/terrain",
    baseColor: 0xffffff
});
```

The function accepts a parameter object with the following properties:

| Parameter       | Required | Description                                                                                                                                                                                |
| --------------- | :------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `api`           |     ✅    | `string`, path to dataset exported from `Metacity`, some sets are available through our [DataAPI](https://api.metacity.cc/), or you can use self-hosted datasets if you have your own data |
| `baseColor`     |     -    | `number`, base color used for mesh and as a default when no styles are applied                                                                                                             |
| `lineColor`     |     -    | `number`, base color used for lines                                                                                                                                                        |
| `loadRadius`    |     -    | `number`, radius around camera target (focus point) where all objects will be loaded                                                                                                       |
| `lodLimits`     |     -    | `number[]`, breakpoints of camera target-position distance to switch LODs                                                                                                                  |
| `name`          |     -    | `string`, name of the layer                                                                                                                                                                |
| `pickable`      |     -    | `boolean`, whether the layer objects should be pickable                                                                                                                                    |
| `pointColor`    |     -    | `number`, base color used for points when no instance models are provided                                                                                                                  |
| `pointInstance` |     -    | `string`, path to GLTF model used for instances                                                                                                                                            |
| `styles`        |     -    | `Style[]`, array of styles applicable to layer                                                                                                                                             |

## Styles

TODO