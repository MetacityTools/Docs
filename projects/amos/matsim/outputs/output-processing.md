# MATSim output processing

Scripts to process MATSim events and network files are available in [this](https://github.com/MetacitySuite/Metacity-MATSimOutput) repository.
The output is movement information in SIM format that can by visualized in Metacity and MATSim network converted to ShapeFile.

## SIM files
The .sim format is our dedicated file format which stores movement information and metadata about dynamic subjects. 

Each file contains a list of dynamic subjects. Continouous movement for each subject is stored as a MultiTimePoint - moovement is defined in time by "start" in seconds, and for each next second are stored the subject coordinates in "geometry". The list of coordinates is encoded as base64 string.


#### Format structure example:
```json
[
    {   "meta": {
            "start": 32093, 
            "subject_id": "18", 
            "passengers": [9], 
            "veh_type": "bike", 
            "vehicle_id": "veh_1_bike"
            },
        "geometry": "ratdTnuaJsF5Vndbnv4vwa2rXU57mibBeVZ3W57+L8EMYQypvJomwQrrzmjV/i/B"
   },
    {   "meta": {
            "start": 35642, 
            "subject_id": "264", 
            "passengers": [245,12], 
            "veh_type": "car", 
            "vehicle_id": "veh_78_car"
            },
        "geometry": "1xWWAs2aJsFL1EX94v4vwaHKH1zdmibBUB6zI+9MO8F2pjMm/v4vwaOb1HwCmybBk/13jQ//L8F1UR1OCpsmwWKCO8oU/y/BgBL6FBWbJsG+YB6XI/8vwZTAB0MdmybBJWDPfDP/L8E="
   }, ...
]
```



If you need to further process the SIM format for your own purposes, you may find the examples below helpful.

### Decoding geometry string back to coordinates
```python
import base64
import numpy as 

def base64_to_float64(b64data):
    bdata = base64.b64decode(b64data)
    data = np.frombuffer(bdata, dtype=np.float64)
    return data

geometry_string = "ratdTnuaJsF5Vndbnv4vwa2rXU57mibBeVZ3W57+L8EMYQypvJomwQrrzmjV/i/B"
coordinates = base64_to_float64(geometry_string)

print("Coordinates decoded:", coordinates)
#output: Coordinates decoded: [ -740669.6530584  -1048399.17864485  -740669.6530584  -1048399.17864485  -740702.33017257 -1048426.70470366]
```


### Projection transformation example
Considering the encoded geometry information uses Krovak projection [epsg:5514](https://epsg.io/5514), you may want to also transform the decoded coordinates under different projection to serve your purposes. Shown is a minimal example:
```python
from pyproj import Transformer
def coord_transform(data = [], epsg_in = 'epsg:5514', epsg_out = 'epsg:4326'):
    transformer = Transformer.from_crs(epsg_in, epsg_out)

    transformed = []
    for x1, y1 in zip(data[0::2],data[1::2]): # data as a list / numpy array of coordinates in Krovak
        transformed.append((transformer.transform(x1, y1)))

    return transformed # as a list of tuples in WGS84

#using coordinates variable from previous example 
print("Coordinates transformed", coord_transform(coordinates)) 
#output: Coordinates transformed [(50.04210434386703, 14.461103605875014), (50.04210434386703, 14.461103605875014), (50.04181924386716, 14.460703805874642)]
```

