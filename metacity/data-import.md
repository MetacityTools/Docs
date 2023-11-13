---
description: Importing data into Metacity
---

# Data Import

Metacity currently supports the following formats:

<table><thead><tr><th width="153.55049298065617">Format</th><th width="150">Suffix</th><th>Reference</th></tr></thead><tbody><tr><td>Shapefile</td><td><code>.shp</code></td><td><a href="https://www.esri.com/content/dam/esrisites/sitecore-archive/Files/Pdfs/library/whitepapers/pdfs/shapefile.pdf">ESRI Shapefile Technical Description</a></td></tr><tr><td>GeoJSON</td><td><code>.json</code></td><td><a href="https://geojson.org">The GeoJSON Specification (RFC 7946)</a></td></tr><tr><td>OBJ</td><td><code>.obj</code></td><td><a href="https://en.wikipedia.org/wiki/Wavefront_.obj_file">OBJ wiki page</a></td></tr></tbody></table>

Importing data is fairly easy with the functionalities provided by the `metacity.io` module:

### Importing a single file

<pre class="language-python"><code class="lang-python">from metacity.io import parse
<strong>#parse(file: str) -> List[Models]
</strong>models = parse("data/file.shp")
</code></pre>

The `parse` function loads contents of a provided file and returns a list of `Models`.

### Importing multiple files

Often, the geospatial data is partitioned into several files and scattered among several directories. If you wish to import all of the data located in the directory tree under a certain folder, you can do:

<pre class="language-python"><code class="lang-python">from metacity.io import parse_recursively
<strong>#parse_recursively(dir: str) -> List[Models]
</strong>models = parse_recursively("data")
</code></pre>

The returned value is a flattened list of `Models` regardless of how many files were processed.
