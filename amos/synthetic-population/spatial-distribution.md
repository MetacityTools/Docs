

# Spatial distribution

To match each census datapoint with corresponding travel survey point, we previously acquired the census data at ZSJ level.
We then created a spatial join between the census data and the available housing with capacity in each ZSJ and randomly assigned residences.
This of course would be better handled had we had household information. 
Travel demands for each census data points are then derived for primary and secondary locations. 
In this case we would greatly benefit from having capacity data for each primary and secondary object.

## Home location
Used data: SHP of address points for Prague
Each residential building in dataset has the average number of inhabitants.


1. Map each residential building onto the corresponding zone.
2. Randomly distribute census inhabitants of each zone between residential buildings.


## Travel demands

### Primary location
Used data: Travel survey data for Prague, commercial and educational object locations

Primary locations (workplace for workers, schools for students) were matched based on available objects in destination ZSJ and corresponding distance for each person from residence to the object.
Locations were matched for each worker and student respondent regardless if they had a primary travel in their activity chain or not.

### Secondary location
Used data: Travel survey data for Prague, amenity and commercial object locations

Secondary locations (shopping, leisure, other) were matched dynamically based on available objects in destination ZSJ and corresponding distance from current and next location in their activity chain.