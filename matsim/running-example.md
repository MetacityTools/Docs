# Running Scenario

### Requirements

1. OpenJDK (11), or any JDK v.11+ (not 16)
2. Python 3.8



## Running the Simulation

1. Clone the Metacity-MATSim repository
2. _Optional: Import the matsim folder into an IDE as a Maven Project_&#x20;
3. Insert input data (MATSim input files: network.xml, plans.xml ... ) into `matsim/data/matsim-files/input` directory
4. Move into the `matsim` folder
5. Run the downsampling of population if needed. The script samples the population xml file: `./downsample.sh 0.1`
6. Create a new MATSim configuration file for running the simulation: `./generate_config.sh`\
   ``Make sure to change the values of parameters in the json property file (e.g. simulation seed, correct population file name...) before running the config generator script. You can add more key-value pairs but make sure it follows the [MATSim configuration file](config-file.md) standard.\
   A new MATSim configuration file with suffix `-run` will be created in `matsim/data/matsim-files`.
7. Run: `./run_matsim.sh`\
   The script defines how much memory will be used for the Java app, change it to your wanted size (option `-Xmx`, e.g. `-Xmx10g` will allocate 10GB RAM)
8. The simulation results will be in `matsim/output` directory by default.

## Preparing MATSim Input Data

todo&#x20;

{% embed url="https://docs.google.com/document/d/1yr5xLs3yGmRyKw3sqACBNbvKVco05fb8R3WCll5Hd8w/edit?usp=sharing" %}
