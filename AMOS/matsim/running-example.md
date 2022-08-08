# Running Scenario

### Requirements

1. OpenJDK (11), or any JDK v.11+ (not 16)
2. Python 3.8
3. MATSim version 13.0+

### Input files

* `pt-network-prague.xml.gz` - multi-modal network with mapped public transit (from OSM + GTFS)
* `population.xml.gz` - population/plans (demand) file with daily plans of agents
* `transit-schedule.xml.gz` - schedule for public transit (from GTFS)
* `transit-vehicles.xml.gz` - description of public transit vehicles (trams, buses, trains)
* `vehicles.xml` - description of non-public vehicles (cars, bikes, ...)
* `config.xml` - configuration file that defines inputs and other simulation parameters (see [Config File](config-file.md) page)

## Running the Simulation

1. Clone the Metacity-MATSim repository
2. _Optional: Import the matsim folder into an IDE as a Maven Project_&#x20;
3. Insert input data (MATSim input files: network.xml, plans.xml ... ) into `matsim/data/matsim-files/input` directory
4. Move into the `matsim` folder
5. ~~Run the downsampling of population if needed. The script samples the population xml file: `./downsample.sh 0.1`~~This step is moved to the SYNPP pipeline (see [Synthetic Population](../synthetic-population/) pages)
6. Create a new MATSim configuration file to run the simulation: `./generate_config.sh`\
   ``Make sure to change the values of parameters in the json property file (e.g. simulation seed, correct population file name...) before running the config generator script. You can add more key-value pairs but make sure it follows the [MATSim configuration file](config-file.md) standard.\
   A new MATSim configuration file with suffix `-run` will be created in `matsim/data/matsim-files`.
7. Build a JAR file using a maven-wrapper: `./mvnw clean package`\
   ``Dependencies are installed into your `/home/yourusername/.m2` directory.\
   The used packages are specified in `matsim/pom.xml` file.
8.  <mark style="background-color:blue;">(a)</mark> Either run: `./run_matsim.sh`\
    The script defines how much memory will be used for the Java app, change it to your wanted size (option `-Xmx`, e.g. `-Xmx10g` will allocate 10GB RAM)

    <mark style="background-color:blue;">(b)</mark> Or run something like: `java -Xmx10g -cp matsim-13.0-v1.0.0-SNAPSHOT.jar org.matsim.run.Controler data/matsim-files/config-prague-run.xml`
9. The simulation results will be in `matsim/output` directory by default.

## Preparing MATSim Input Data

All these following steps are prepared in the `CreateMatsimNetwork.java`, and can be run directly given that OSM-Network and mapping configuration files are located in `matsim/data/matsim-files` directory. All the input properties used in creating the network are listen in `matsim/resources/config.properties`, including the EPSG [coordinates](coordinates.md), GTFS sample day etc.

### Network

Before running the simulation, a MATSim network must be created.&#x20;

First, a multimodal network has to be created (roads for cars, buses, tram, rail, ...). The network is created using the OSM (see [here](osm.md)). The conversion is controlled using the OSM-Network configuration file [<mark style="background-color:green;">osm-network-config-file.xml</mark>](https://github.com/MetacitySuite/Metacity-MATSim/blob/main/matsim/data/matsim-files/osm-network-config-file.xml).&#x20;

The OSM road structure is projected into coordinates in which the Euclidean distance is valid.

* **Output**: a multimodal network in MATSim XML format (probably zipped as well)

#### <mark style="background-color:green;">OSM-Network configuration file</mark>

Parameters involving paths (and need to be changed accordingly):

* `osmFile` - path to the OSM file
* `outputNetworkFile` - output file for the multimodal network in MATSim XML format

### Public Transit

The GTFS for the public transit is then mapped onto the network. Click [here](filtering-gtfs.md) to see how to filter out routes outside the OSM area. The mapping is configured using the [<mark style="background-color:purple;">pt-mapping-config.xml</mark>](https://github.com/MetacitySuite/Metacity-MATSim/blob/main/matsim/data/matsim-files/pt-mapping-config.xml) mapping configuration file.&#x20;

The links of the network have \[x, y] projected coordinates specified, meanwhile the GTFS is in WGS84 and the route is not given - only the order of stops along the route are known. Therefore, in this step the transit schedules are mapped onto the MATSim multimodal network and so the public transport is merged with the MATSim network.

* **Output**: a multimodal network with mapped public transit routes, transit vehicles (bus, tram vehicles, ...) and a mapped transit schedule (timetable) for a chosen GTFS sample day

#### <mark style="background-color:purple;">PT mapping configuration file</mark>

Parameters involving paths (and need to be changed accordingly):

* `inputNetworkFile` - path to the multimodal network in MATSim XML format
* `inputScheduleFile` - intermediate schedule file created when processing GTFS
* `outputNetworkFile` - a multimodal network with mapped public transit routes
* `outputScheduleFile` - a mapped transit schedule (timetable)

#### Mapping Options

For modes such as subway or ferry, artificial links are created - they do not have a designated road in the OSM file.&#x20;

The routes used by tram can be defined in the mapping configuration file <mark style="background-color:purple;">pt-mapping-config.xml</mark>. If no network modes are defined, the transit route will use artificial links.

```
<parameterset type="transportModeAssignment" >
	<param name="networkModes" value="tram" /> <!-- roads in OSM with a tag "tram" -->
	<param name="scheduleMode" value="tram" />
</parameterset>
```

As of now the trams are also in their own network, not driving alongside cars or buses. Please see: [https://github.com/matsim-org/matsim-code-examples/issues/397#issuecomment-658866756](https://github.com/matsim-org/matsim-code-examples/issues/397#issuecomment-658866756)

### Scaling Vehicle PCE

When running a scaled-down simulation, the PCE for buses have to be re-defined, otherwise the buses get stuck.

For example simulating 10% of population results in **bus PCE = 0.3**. This can be edited in file `transit-vehicles.xml.gz`&#x20;

```
<vehicleType id="Bus">
	<capacity>
		<seats persons="70"/>
		<standingRoom persons="0"/>
	</capacity>
	<length meter="18.0"/>
	<width meter="2.5"/>
	<accessTime secondsPerPerson="0.5"/>
	<egressTime secondsPerPerson="0.5"/>
	<doorOperation mode="serial"/>
	<passengerCarEquivalents pce="0.3"/> <!-- edit here -->
</vehicleType>
```

### Vehicles.xml

<mark style="color:red;">Different from</mark> <mark style="color:red;"></mark><mark style="color:red;">`transit-vehicles.xml`</mark> <mark style="color:red;"></mark><mark style="color:red;">which is for public transit (bus, tram, ...)!</mark>

See: [https://github.com/matsim-org/matsim-libs/tree/master/examples/scenarios/equil-mixedTraffic](https://github.com/matsim-org/matsim-libs/tree/master/examples/scenarios/equil-mixedTraffic)

Specifies a type of car, bike, ... that the agents use

* either by type of the vehicle - `modeVehicleTypesFromVehiclesData`
* or individually for each agent (by id) - `fromVehiclesData`

If this file is used, the `config.xml` file must include the parameter `vehiclesSource`:

```
<module name="qsim">
    ...
    <param name="vehiclesSource" value="modeVehicleTypesFromVehiclesData" /> 
</module>

...

<module name="vehicles" >
	<param name="vehiclesFile" value="./input/vehicles.xml" /> <!-- path to the vehicle.xml file relative to the config.xml file -->
</module>

```


## Transport Modes

* Main mode (network mode) - these modes use routing algorithm on the provided MATSim network (with e. g. "qsim" algorithm)
  * car, (bike <mark style="color:red;">\*</mark>), ...
* Transit mode - the modes have predefined routes according to the transit schedule, which do not change during the simulation
  * all public transit (pt): bus, tram, subway, ferry, funicular, ...
* Teleport mode - these modes do not use any routing in the MATSim network, they only take into account the start and end link of an activity a "teleport" along the beeline in a predefined time or speed factor
  * walk, bike, ...

<mark style="color:red;">\*</mark> <mark style="color:purple;"></mark> Bikes may be simulated in the main mode if the network has a well connected network for bikes (as in there always exists a way from any node x to any node y)

See mixed traffic settings in MATSim: [https://matsim.atlassian.net/wiki/spaces/MATPUB/pages/84246576/Mixed+traffic](https://matsim.atlassian.net/wiki/spaces/MATPUB/pages/84246576/Mixed+traffic)

