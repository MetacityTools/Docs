# Case Studies

## Sao Paulo

[Article](https://www.research-collection.ethz.ch/bitstream/handle/20.500.11850/429951/ab1545.pdf?sequence=1\&isAllowed=y) [GitHub](https://github.com/eqasim-org/sao\_paulo/blob/master/docs/howto.md)

### Census Data

[data](https://centrodametropole.fflch.usp.br/en/download-de-dados?f%5B0%5D=facets\_temas%3Ademographic%20census%202010\&f%5B1%5D=facets\_tipos%3Ano%20cartographic\&busca\_geral=\&items\_per\_page=20)

Sociodemographic information of people and households (HH) - scaled to 10%

1.  Cleaning - raw.py, cleaned.py

    1. Orig - raw

    ```
            df_census.columns = ["federationCode", "areaCode", "householdWeight", "metropolitanRegion", "personNumber", "gender", "age", "goingToSchool", "employment", "onLeave", "helpsInWork", "farmWork", "householdIncome", "motorcycleAvailability", "carAvailability", "numberOfMembers"]
    ```

    1. Spatial editing

    ```
            df_census["zone_id"] = df_census["areaCode"] 
    ```

    1. cleaned

    ```
            df = df[["person_id", "household_id","weight","zone_id","residence_area_index","age", "sex", "employment", "binary_car_availability","household_size", "household_income"]]
    ```
2. Entry HHi is multiplicated by weight wi (which indicates how many households a specific entry represents, use stochastic rounding for floats) \~ “copy” each household wi times
   1. Mind the sampling rate (direct or not)
3. Income information is in the census - divided into bins

### Zones

[data](https://drive.google.com/file/d/1TdXEfwruBDYorcf9E7yG8kPI6yMfjV5O/view) OSM, zones, schools (shapefiles, csv)

From zones in shapefiles (use geopandas, set current coordinate system crs, transform to different coord system), set zone id

```
    df_zones_census_dissolved = df_zones_census_dissolved[['geometry', 'AP_2010_CH']]
    df_zones_census_dissolved.columns = [ "geometry", "zone_id"]
```

1. Extract roads from OSM: x, y, purpose
2. Create “opportunities” - offer work, offer houses - transform geometries to a new coordinate reference system (geopandas)
3.  Add schools to opportunities - transform geometries to a new coordinate reference system (geopandas)

    1.

    ```
        df_facilities_education["offers_work"] = True
    ```

    1.

    ```
        df_facilities_education["offers_other"] = True
    ```
4. From shapefile of zones - transform geometries to a new coordinate reference system (geopandas): geometry, zone\_id

### HTS

[data](https://transparencia.metrosp.com.br/dataset/pesquisa-origem-e-destino/resource/4362eaa3-c0aa-410a-a32b-37355c091075) household travel survey \~ contains 84 889 samples which are weighted, so that the total weight sum amounts to 20 508 979, more or less the number of inhabitants in the area in 2017.

1. Cleaning (removing NaN, duplicates), remapping categories
2. Divide into two dataframes - person, trips
3. Remapping categories (work - employed, not employed, student; trip purpose - home, leisure, shop, work; mode - pt, car, car-passenger...)
4.  Create point from home\_coord for each person + remapping to correct coordinate system

    1.

    ```
        df_persons["geometry"] = [geo.Point(*xy) for xy in zip(df_persons["homeCoordX"], df_persons["homeCoordY"])]
    ```

    1.

    ```
        df_geo = gpd.GeoDataFrame(df_persons, crs = {"init" : "EPSG:29183"})
    ```
5. Map home coords (points) of each person to zones (poly) -> home\_zones

```
    home_zones = gpd.sjoin(df_geo[["person_id","geometry"]], df_zones[["zone_id","geometry"]], op = "within",how="left")
```

1.  Generating areas 1.

    ```
    sp_area = [3 * (z in center) + 2 * (z in city and z not in center) + 1 * (z in region and z not in city) for z in zone_id] 
    ```

    1.

    ```
    df_persons["residence_area_index"] = sp_area
    ```
2. Generating trips
   1. origin and destination purpose (shop, work, ...), mode, zones, …
   2. Remove trips from a place to the same place
   3. Remove trips not starting at home, remove trips not ending at home
   4. Calculate activity duration
   5. Spatial join origin & destination coords with zones

#### Output

Persons.csv - "person\_id", "weight", "age", "sex", "employment", "binary\_car\_availability", "has\_pt\_subscription", "home\_zone", "household\_income", "residence\_area\_index", "homeCoordX", "homeCoordY"

Trips.csv - "person\_id", "trip\_id", "new\_trip\_id", "preceeding\_purpose", "following\_purpose", "mode","departure\_time", "arrival\_time", "crowfly\_distance", "activity\_duration", "origin\_x", "origin\_y", "destination\_x", "destination\_y", "origin\_zone", "destination\_zone", "homeCoordX", "homeCoordY"

### OD matrices

An origin–destination matrix is a matrix in which each cell represents the number of trips from an origin zone (given by the corresponding row of the matrix) to a destination zone (column), or the percentage of trips starting in the origin zone that reach the destination zone. Those matrices can be created from the household travel survey. In this study, one weighted origin–destination matrix was generated for work trips.

## Paris and Ile-de-France

[Article](https://www.sciencedirect.com/science/article/pii/S0968090X21003016) [GitHub](https://github.com/eqasim-org/ile-de-france/blob/develop/docs/population.md)

### Data

* Spatial zoning system
* Census data
  * Large microsample of the population - 30% households in France
  * Commuting relations (flow matrix, moving for work and education purpose) between municipalities
  * Aggregated zonal information
* Household income
* Household travel survey
  * Detailed activity chain for one reference person (what activities, when, how the person moved between activities)
* Locations at which activities can take place - address, coordinates,
  * Work - number of employees
  * Location of educational facilities

1. socio demographic information and residence locations of households and persons are generated - municipality or \[area] is defined for each synthetic person
2. income information is added to each household
3. activity chains are attached to the synthetic persons - statistical matching based on the correlation of daily activity patterns and sociodem. attributes
4. places of work and education are assigned - primary location assignment, using commuting matrix
   1. Correct number of people should commute from one municipality to another in the population
   2. The commute distance should fit the assigned activity chain
5. the locations of all other activities in the persons’ activity chains are chosen

## Ústí nad Labem

[https://www.researchgate.net/publication/357836049\_Synthetic\_Population\_Generator\_for\_Activity-Based\_Travel\_Demand\_Models](https://www.researchgate.net/publication/357836049\_Synthetic\_Population\_Generator\_for\_Activity-Based\_Travel\_Demand\_Models)
