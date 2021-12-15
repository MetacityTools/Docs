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
7. Build a JAR file using a maven-wrapper: `./mvnw clean package`\
   ``Dependencies are installed into your `/home/.m2` directory.\
   The used packages are specified in `matsim/pom.xml` file.
8. Run: `./run_matsim.sh`\
   The script defines how much memory will be used for the Java app, change it to your wanted size (option `-Xmx`, e.g. `-Xmx10g` will allocate 10GB RAM)
9. The simulation results will be in `matsim/output` directory by default.

## Preparing MATSim Input Data

### Network

Before running the simulation, a MATSim network must be created.&#x20;

First, a multimodal network has to be created (roads for cars, buses, tram, rail, ...). The network is created using the OSM (see [here](osm.md)). The conversion is controlled using the OSM-Network configuration file <mark style="background-color:orange;">osm-network-config-file.xml</mark>.&#x20;

* Output: multimodal network in MATSim XML format (probably zipped as well)

Then the GTFS for the public transit is mapped onto this network. Click [here](filtering-gtfs.md) to see how to filter out routes outside the OSM area. The mapping is configured using the <mark style="background-color:orange;">pt-mapping-config.xml</mark> mapping file.

* Output:&#x20;

All these steps are prepared in the `CreateMatsimNetwork.java`, and can be run directly given that OSM-Network and mapping configuration files are located in `matsim/data/matsim-files` directory. All the input properties used in creating the network are listen in `matsim/resources/config.properties`, including the EPSG coordinates, GTFS sample day etc.





{% embed url="https://docs.google.com/document/d/1yr5xLs3yGmRyKw3sqACBNbvKVco05fb8R3WCll5Hd8w/edit?usp=sharing" %}
