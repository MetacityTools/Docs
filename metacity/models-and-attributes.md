---
description: Using Metacity's models and geometry attributes
---

# Models and Attributes

A `Model` is a universal entity for storing geometry and metadata, it has no specific semantic meaning.&#x20;

* The geometry is stored in `Attributes`. Models can have multiple `Attributes` - see section [Attributes](models-and-attributes.md#attributes).
* The properties can be attached to a model as a metadata

See the following example:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Model, Attribute

model = Model()
position_attr = Attribute()
position_attr.push_polygon3D([[0, 0, 0, \
                               0, 0, 1, \
                               0, 1, 1]])
                          
<strong>#Model.add_attribute(self, attr_name: str, attr: Attribute) -> None
</strong>model.add_attribute("POSITION", position_attr)

<strong>#Model.set_metadata(self, metadata: dict) -> None
</strong>model.set_metadata({ "description": "An example triangle" })
</code></pre>

Additionally, the `Model` class has the following methods:

<pre class="language-python"><code class="lang-python">#Complementary to method add_attribute, there is also 
<strong>#Model.attribute_exists(self, attr_name: str) -> bool
</strong>assert model.attribute_exists("POSITION") == True

<strong>#Model.get_attribute(self, attr_name: str) -> Attribute
</strong>position_attr = model.get_attribute("POSITION")
</code></pre>

Note that the property `Model.metadata` returns a copy. Updating it won't affect the metadata stored inside the model. If you wish to update the metadata, use the `Model.set_metadata` method:

<pre class="language-python"><code class="lang-python"><strong>#@property
</strong><strong>#Model.metadata(self) -> dict
</strong>model.metadata["description"] = "A new description"
assert model.metadata["description"] != "A new description"

#updating the metadata
<strong>#Model.set_metadata(self, metadata: dict) -> None
</strong>model.set_metadata({ "description": "A new description" })
assert model.metadata["description"] == "A new description"
</code></pre>

It is possible to check what is the geometry type of the `Model`. Note that models do not support mixing geometry types. If you need this feature, consider splitting your geometry into several `Models`.&#x20;

<pre class="language-python"><code class="lang-python"><strong>#@property
</strong><strong>#Model.geom_type(self) -> int
</strong>geometry_type_code = model.geom_type
</code></pre>

For the encoding explanation, see Attribute [Geometry Type](models-and-attributes.md#geometry-type). The `geom_type` property of the `Model` class always returns the type of the `POSITION` attribute.&#x20;

### Attributes&#x20;

`Attributes` work similarly to [GLTF buffers](https://www.khronos.org/files/gltf20-reference-guide.pdf). The `Attribute` API allows parsing of various data. All data is stored inside as if it was 3D data (2D data gets padded by zeroes).&#x20;

A unique name identifies each attribute. There are a few names that are commonly used for certain types of attributes:

| Name       | Description                         |
| ---------- | ----------------------------------- |
| `POSITION` | coordinates of the vertex positions |
| `NORMAL`   | normal buffer                       |
| `COLOR`    | vertex color                        |

`Metacity` does not utilize indices for geometry reuse, all data is stored "as is" in sequential order.

If everything works as intended, a user **should rarely work directly with** `Attributes`. Occasionally, it might be useful to be able to access the Attribute API:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Attribute

points = Attribute()
#Insert 2D points using push_line2D method:
<strong>#Attribute.push_point2D(self, points: List[float]) -> None
</strong>points.push_point2D([0, 0, \
                    1, 1, \
                    1, 2])
#Similar for 3D points
<strong>#Attribute.push_point3D(self, points: List[float]) -> None
</strong>points.push_point3D([0, 0, 0,   \
                     1, 1, 0.5, \
                     1, 2, 1])
                     
<strong>#@property
</strong><strong>#Attribute.geom_type(self) -> int:
</strong>assert points.geom_type == 1
</code></pre>

Parsing Lines is very similar to parsing points, although the main difference is the data gets stored as individual segments duplicating inner vertices.  &#x20;

<pre class="language-python"><code class="lang-python">from metacity.geometry import Attribute

lines = Attribute()
#Insert 2D line string using push_line2D method:
<strong>#Attribute.push_line2D(self, line: List[float]) -> None
</strong>lines.push_line2D([0, 0, \
                    1, 1, \
                    1, 2])
#in the example above, the vertices get internally stored as:
#input:          a - b - c
#stored values: [0, 0, 0, 1, 1, 0, 1, 1, 0, 1, 2, 0]
#               [a - b, b - c]
#               every 3rd zero is padding for 3D data, inner vertex si duplicated 

#Similar for 3D points
<strong>#Attribute.push_line3D(self, line: List[float]) -> None
</strong>lines.push_line3D([0, 0, 0,   \
                    1, 1, 0.5, \
                    1, 2, 1])
                    
<strong>#@property
</strong><strong>#Attribute.geom_type(self) -> int:
</strong>assert lines.geom_type == 2
</code></pre>

Polygons are automatically triangulated, the API also supports polygons with holes:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Attribute

triangles = Attribute()
#Insert simple 2D polygon using push_polygon3D method:
<strong>#Attribute.push_polygon2D(self, polygon: List[List[float]]) -> None
</strong>triangles.push_polygon2D([[0, 0, \
                          0, 1, \
                          1, 1, \
                          0, 1]])
                          
#The structure draws from GeoJSON specs,
#the List[List[float]] can be interpreted as [[polygon], [hole], [hole] ...] 
#Insert 2D polygon with a hole in the middle:
triangles.push_polygon2D([[0, 0, \
                          0, 1, \
                          1, 1, \
                          0, 1],\
                          [0.25, 0.25, \
                           0.25, 0.75, \
                           0.75, 0.75, \
                           0.75, 0.25])

#All works equivalently for 3D:
<strong>#Attribute.push_polygon3D(self, polygon: List[List[float]]) -> None    
</strong>triangles.push_polygon3D([[0, 0, 1, \
                          0, 1, 1, \
                          1, 1, 1, \
                          0, 1, 1]])     
                          
<strong>#@property
</strong><strong>#Attribute.geom_type(self) -> int:
</strong>assert triangles.geom_type == 3                  
</code></pre>

### Geometry Type

Notice the `geom_type` property:

<pre class="language-python"><code class="lang-python">from metacity.geometry import Attribute
attribute = Attribute()
#...
<strong>#@property
</strong><strong>#Attribute.geom_type(self) -> int:
</strong>assert attribute.geom_type == 0 #No geometry
assert attribute.geom_type == 1 #Points
assert attribute.geom_type == 2 #Lines
assert attribute.geom_type == 3 #Triangles
</code></pre>

### Attribute Caveats

As demonstrated above, `Attributes` can handle loading various types of data. What you should never do is _mix_ those types of data:

```python
#!!!NEVER DO THIS!!!
from metacity.geometry import Attribute
points = Attribute()
points.push_point2D([0, 0, 1, 1, 1, 2])
points.push_line2D([0, 0, 1, 1, 1, 2]) #<- will raise exception
```

What you can do is mix 2D and 3D data since 2D gets internally padded by zeroes:

```python
from metacity.geometry import Attribute
points = Attribute()
points.push_point2D([0, 0, 1, 1, 1, 2])
points.push_point3D([0, 0, 0, 1, 1, 0, 1, 2, 0]) #<- is allowed
```

{% hint style="warning" %}
Internally, `Attribute` keeps track of the first assigned type (points, lines, or polygons) and checks all future assignments against it. If the type does not match, an exception gets raised.
{% endhint %}
