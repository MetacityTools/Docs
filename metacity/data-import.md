---
description: Importing data into Metacity
---

# Data Import

Metacity currently supports the following formats:

| Format    | Suffix  | Reference                                                                                                                                             |
| --------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Shapefile | `.shp`  | [ESRI Shapefile Technical Description](https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf) |
| GeoJSON   | `.json` | [The GeoJSON Specification (RFC 7946)](https://geojson.org)                                                                                           |
| OBJ       | `.obj`  | [OBJ wiki page](https://en.wikipedia.org/wiki/Wavefront\_.obj\_file)                                                                                  |

Importing data is fairly easy with the functionalities provided by the `metacity.io` module:

### Importing a single file

<pre class="language-python"><code class="lang-python">from metacity.io import parse
<strong>#parse(file: str) -> List[Models]
</strong>models = parse("data/file.shp")</code></pre>

The `parse` function loads contents of a provided file and returns a list of `Models`.

### Importing multiple files

Often, the geospatial data is partitioned into several files and scattered among several directories. If you wish to import all of the data located in the directory tree under a certain folder, you can do:

<pre class="language-python"><code class="lang-python">from metacity.io import parse_recursively
<strong>#parse_recursively(dir: str) -> List[Models]
</strong>models = parse_recursively("data")</code></pre>

The returned value is a flattened list of `Models` regardless of how many files were processed.
