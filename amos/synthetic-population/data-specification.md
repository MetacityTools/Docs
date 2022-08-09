---
description: Definition and preprocessing of available and necessary data to create a synthetic population.
---
# Data Specification

# Available data (as of 7. 12. 2021):
* **SLDB 2011** - Full census (Praha + stredni Cechy) 
* **Česko v Pohybu** - travel diary (2017-2019)

SHP files (2021)
* Population address point - residences with average number of residents in Prague
* Amenities - commercial, civic, recreational POI - [x, y] coordinates
* Zones in Prague - ZSJ (základní sídelní jednotka) - 687 zones

Additional files 
* Mapping of ZSJ to districts (Praha 1-22, ...) - 57 districts
* Mapping between districts (census (57) <-> travel diary (cca 30) districts)
* Probability of commute between zones to work or education (from SLDB 2011)

# Pipeline overview
1. Clean census -> one census person = individual of the synthetic population
2. Clean travel diary -> travelers with travel day plan and district of residence
3. Statistical match of a census person with a traveler (based on age, sex, employment, and district of residence) -> individual has an activity chain (travelling plan for a day) copied from the matched traveler
4. Assign home coordinates to each individual based on zone of residence -> individual has a home location based on which all other coordinates for other destinations are determined
5. Assign coordinates to destinations (work, education, shop, other, leisure) for each individual based:
    1. Amenities
    2. Travel mode
    3. Trip duration
    4. Beeline distance 
    5. Probability of commute between zones to work or education
    6. Home coordinates
6. Convert to MATSim XML population file with travel demands

# Data Preprocessing

## SLDB
entries: 2551962
Prague entries: 1268463

Steps:
1. Filter by region 3018
2. Drop NA in ['employment', 'sex', 'zone_id']
3. Fill age median grouped by ['sex', 'employment']
4. Fill most frequent employment grouped by ['sex', 'age']


## Česko v Pohybu
* Households: # entries 1509
* Travelers: # 3418
* Trips: # 8219

### Households
Steps:
    Sum # of cars (private, company, utility, other)) in car_number

### Travelers
Steps:
1. Drop NA in ['sex', 'employment']
2. Fill age median grouped by ['sex', 'employment']
3. Fill NA using sociodemographics
    - If age > 18 and driving_license is NA -> driving_license = True
    - If age < 18 and driving_license is NA -> driving_license = False
    - If age < 16 or age > 64 -> pt_avail = True

### Trips
Steps:
1. Drop NA in [traveler_id]
2. Drop NA in [departure_h, departure_m, arrival_h, arrival_m, origin_purpose, destination_purpose, traveling_mode]
3. Delete travelers who do not have any destination ‘home’
4. Keep only travelers where ‘*_purpose’ == ‘home’ and ‘*_code’ == 1000
5. Fill NA in ‘*_code’ for missing commute home-work, home-education (and vice versa)
6. Impute driving mode "other" by mode of groupby by traveling speed.
