---
description: 3D Web Visualization Library
---

# 🗺 BananaGL

Visualize data preprocessed by `Metacity` with [`BananaGL`](https://github.com/MetacitySuite/BananaGL) - a client-side web visualization library. It is built with [three.js](https://threejs.org/) and `Typescript`.&#x20;

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
    <div id="container">
        <canvas id="canvas">Your browser does not support canvas</canvas>
    </div>
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

{% hint style="warning" %}
**Never** set any **CSS styles** (directly or in a separate style sheet) modifying **width or height of the`<canvas>`** passed to BananaGL. **** Set the sizes on its **parent** instead.
{% endhint %}

`BananaGL` keeps the provided `<canvas>` at 100% width and height of its parents, and adds a small annotation to its bottom.&#x20;

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
| `lightIntensity`               |     -    | `number`, total intensity of lights                                                                                                                                  |

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

| Parameter       | Required | Description                                                                                                                                                                                                                                               |
| --------------- | :------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `api`           |     ✅    | `string`, path to dataset exported from `Metacity` (see [Exporting data](../../tools/metacity.md#exporting-data)), some sets are available through our [DataAPI](https://api.metacity.cc/), or you can use self-hosted datasets if you have your own data |
| `baseColor`     |     -    | `number`, base color used for mesh and as a default when no styles are applied                                                                                                                                                                            |
| `lineColor`     |     -    | `number`, base color used for lines                                                                                                                                                                                                                       |
| `loadRadius`    |     -    | `number`, radius around camera target (focus point) where all objects will be loaded                                                                                                                                                                      |
| `lodLimits`     |     -    | `number[]`, breakpoints of camera target-position distance to switch LODs                                                                                                                                                                                 |
| `name`          |     -    | `string`, name of the layer                                                                                                                                                                                                                               |
| `pickable`      |     -    | `boolean`, whether the layer objects should be pickable                                                                                                                                                                                                   |
| `pointColor`    |     -    | `number`, base color used for points when no instance models are provided                                                                                                                                                                                 |
| `pointInstance` |     -    | `string`, path to GLTF model used for instances                                                                                                                                                                                                           |
| `styles`        |     -    | `Style[]`, array of styles applicable to layer                                                                                                                                                                                                            |

## Styles

The color of mesh models can be modified by providing styling rules to individual layer.

```javascript
const gl = new BananaGL.BananaGL({...});

gl.loadLayer({
    api: "https://data.metacity.cc/buildings",
    baseColor: 0xffffff,
    style: [
        gl.style.withAttributeEqualTo("utilization", "office").useColor(0x0000ff),
        gl.style.withAttributeEqualTo("height", 50).useColor(0xff0000)
    ]
});
```

In the example above, the object with attribute `utilization` set to value `office` will be blue, and all buildings with `height` equal to 50 will be red.

### Computed Properties

On loading, all mesh objects are automatically assigned the following attributes:

* `height` - `number`, height of the object
* `baseHeight` - `number`, minimal z-coordinate of all object vertices
* `bbox` -  `[number`\[`], number[]]`, object bounding box, `bbox[0]` is the minimal coordinate , `bbox[1]` is maximum

### Rule Resolution

If two or more rules apply to a single object, the last matched rule is applied.&#x20;

If no rule matches, the `baseColor` of the layer is applied.

### Rule Chaining

If you want to specify a rule where both of the rules must apply, you can chain them in a following way:

```javascript
const gl = new BananaGL.BananaGL({...});

gl.loadLayer({
    api: "https://data.metacity.cc/buildings",
    baseColor: 0xffffff,
    style: [
        gl.style.withAttributeEqualTo("utilization", "office")
                .withAttributeEqualTo("height", 50).useColor(0xffe135)
    ]
});
```

## Style Rules

Several styling rules are available:

### `.forAll`

Applies the same color to all objects, essentially the same as using `baseColor` (just more computationally expensive, use only for testing or if you really despise your users).

```javascript
gl.style.forAll().useColor(0xffe135)
```

### `.withAttributeEqualTo`

Applies color to all objects with a certain attribute equal to a provided value.

```javascript
gl.style.withAttributeEqualTo("usage", "restaurant").useColor(0xffe135)
```

### `.withAttributeRange`

Applies color to all objects with a certain attribute in provided range.&#x20;

```javascript
gl.style.withAttributeRange("height", 10, 20).useColor(0xffe135)
gl.style.withAttributeRange("height", 10, 20).useColor([0x000000, 0xffffff])
```

### `.withAttributeRangeExt`

Applies color to all objects with a certain attribute in provided range. If the value is outside of the range, the color is extrapolated (constant method).

Combined with `useColor` using a single color works as an indication of whether the attribute is present.

```javascript
gl.style.withAttributeRangeExt("height", 10, 20).useColor(0xffe135)
gl.style.withAttributeRangeExt("height", 10, 20).useColor([0x000000, 0xffffff])
```

### `.useColor`

Specifies which color or colormap should be used if this particular rule applies. You can specify a single color

```javascript
gl.style.withAttributeEqualTo("usage", "restaurant").useColor(0xffe135)
```

or a color palette. Palettes work well in combination with `AttributeRange` rules.

```javascript
gl.style.withAttributeRangeExt("height", 10, 20).useColor([0x000000, 0xffffff])
```

If you call `useColor` more than once on a single rule, the last provided value is used.

```javascript
gl.style.withAttributeEqualTo("usage", "restaurant")
        .useColor(0xffe135) //<- not used
        .useColor(0xffffff) //<- used
```
