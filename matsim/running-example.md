# Running Scenario

## Requirements

1. OpenJDK (11), or any JDK v.11+ (not 16)
2. Python 3.8
3. R (for filtering GTFS)

## Running the Simulation

1. Clone the Metacity-MATSim repository
2. Insert input data (MATSim input files: network.xml, plans.xml ... ) into matsim/data/matsim-files/input directory
3. From inside the matsim folder run downsample.sh if needed. The script samples the population xml file. ./downsample.sh 0.1
4.  Create a new MATSim configuration file for running the simulation: generate\_config.sh

    Make sure to change values of parameters in the json property file (e.g. simulation seed, correct population file name...) before running the config generator.
5. Run ./run\_matsim.sh
6. The simulation results will be in matsim/output directory.

{% embed url="https://docs.google.com/document/d/1yr5xLs3yGmRyKw3sqACBNbvKVco05fb8R3WCll5Hd8w/edit?usp=sharing" %}
