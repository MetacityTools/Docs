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

All these following steps are prepared in the `CreateMatsimNetwork.java`, and can be run directly given that OSM-Network and mapping configuration files are located in `matsim/data/matsim-files` directory. All the input properties used in creating the network are listen in `matsim/resources/config.properties`, including the EPSG [coordinates](coordinates.md), GTFS sample day etc.

### Network

Before running the simulation, a MATSim network must be created.&#x20;

First, a multimodal network has to be created (roads for cars, buses, tram, rail, ...). The network is created using the OSM (see [here](osm.md)). The conversion is controlled using the OSM-Network configuration file <mark style="background-color:green;">osm-network-config-file.xml</mark>.&#x20;

The OSM road structure is projected into coordinates in which the Euclidean distance is valid.

* **Output**: a multimodal network in MATSim XML format (probably zipped as well)

### Public Transit

The GTFS for the public transit is then mapped onto the network. Click [here](filtering-gtfs.md) to see how to filter out routes outside the OSM area. The mapping is configured using the <mark style="background-color:green;">pt-mapping-config.xml</mark> mapping configuration file.&#x20;

The links of the network have \[x, y] projected coordinates specified, meanwhile the GTFS is in WGS84 and the route is not given - only the order of stops along the route are known. Therefore, in this step the transit schedules are mapped onto the MATSim multimodal network and so the public transport is merged with the MATSim network.

* **Output**: a multimodal network with mapped public transit routes, transit vehicles (bus, tram vehicles, ...) and a mapped transit schedule (timetable) for a chosen GTFS sample day

#### Mapping Options

For modes such as subway or ferry, artificial links are created - they do not have a designated road in the OSM file.&#x20;

The routes used by tram can be defined in the mapping configuration file <mark style="background-color:green;">pt-mapping-config.xml</mark>. If no network modes are defined, the transit route will use artificial links.

```
<parameterset type="transportModeAssignment" >
	<param name="networkModes" value="tram" /> <!-- roads in OSM with a tag "tram" -->
	<param name="scheduleMode" value="tram" />
</parameterset>
```

As of now the trams are also in their own network, not driving alongside cars or buses. Please see: [https://github.com/matsim-org/matsim-code-examples/issues/397#issuecomment-658866756](https://github.com/matsim-org/matsim-code-examples/issues/397#issuecomment-658866756)

#### Public Transit References

* [https://github.com/matsim-org/pt2matsim/wiki](https://github.com/matsim-org/pt2matsim/wiki)
* [https://github.com/matsim-org/matsim-code-examples/wiki/pt](https://github.com/matsim-org/matsim-code-examples/wiki/pt)
* [https://matsim.atlassian.net/wiki/spaces/MATPUB/pages/83099693/Transit+Tutorial?focusedCommentId=151191553](https://matsim.atlassian.net/wiki/spaces/MATPUB/pages/83099693/Transit+Tutorial?focusedCommentId=151191553)
* [https://d-nb.info/1066161852/34 ](https://d-nb.info/1066161852/34)
* [https://www.researchgate.net/publication/317039686\_MATSim\_simulations\_in\_the\_Tel\_Aviv\_Metropolitan\_Area\_Direct\_competition\_between\_public\_transport\_and\_cars\_on\_the\_same\_roadway](https://www.researchgate.net/publication/317039686\_MATSim\_simulations\_in\_the\_Tel\_Aviv\_Metropolitan\_Area\_Direct\_competition\_between\_public\_transport\_and\_cars\_on\_the\_same\_roadway)
* [https://jitpack.io/p/colinsheppard/pt2matsim](https://jitpack.io/p/colinsheppard/pt2matsim)
* [https://ethz.ch/content/dam/ethz/special-interest/baug/ivt/ivt-dam/publications/students/501-600/sa530.pdf](https://ethz.ch/content/dam/ethz/special-interest/baug/ivt/ivt-dam/publications/students/501-600/sa530.pdf)

