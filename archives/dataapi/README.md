---
description: Tile service providing GLTF models for BananaGL library
---

# DataAPI

The service frontend is available at [api.metacity.cc](https://api.metacity.cc).

As this is a service, it consists of several components:

* The **data storage** is just a _good old_ filesystem on our server. It contains folders of data exported from `Metacity` - see [Exporting data](../../tools/metacity.md#exporting-data)
* The catalog **frontend** is a small Flask application called [DataAPI](https://github.com/MetacitySuite/DataAPI)
* The data is **served** by Nginx - for more details, see [Development page](development.md)

## Publishing Data

Currently, we need to upload the processed data to the server manually. If you are interested in us opening the access to others, [get in touch with us](mailto:hello@metacity.cc)!

## Data API Frontend

The frontend is a small Flask application, which scans the filesystem on request and presents a list of found datasets. It operates based on a simple config:&#x20;

```json5
{
    //root directory, where the dataset directories are stored
    "datasetDir": "/home/monika/data", 
    //url to prepend before each directory name 
    "rootAddress": "https://data.metacity.cc/"
}
```

### Dataset Descriptions

It is possible to add a more detailed description to each datasets by placing a `description.json` file into the dataset directory next to the obligatory `layout.json`. The json gets rendered on the frontend above the dataset url.









