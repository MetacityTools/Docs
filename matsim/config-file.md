---
description: The config file influences how the MATSim simulation behaves.
---

# Config File

## Default Configuration File

The list of available parameters and valid parameter values may vary from release to release. For a list of all available settings available with the version you are working with, run "DefaultConfig.java" in the Metacity-MATSim module.

## Configuration File

The configuration file consists of _modules._ Each module represents a logical parameter group. Numerous MATSim modules can be added to MATSim and configured by specifying the respective configuration file section.

## Modules

List of modules to configure in our Prague scenario

### Global

```
randomSeed
coordinateSystem
numberOfThreads - for parallel event handling
insistingOnDeprecatedConfigVersion
```

### Controler

```
outputDirectory
firstIteration
lastIteration
compressionType
dumpDataAtEnd
routingAlgorithmType - the Fast versions takes up more memory
overwriteFiles
createGraphs
writeEventsInterval
writePlansInterval
writeSnapshotsInterval
writeTripsInterval
```

### QSim

```
startTime
endTime - should be specified so the simulation does not run forever for agents with faulty plans
flowCapacityFactor - scales the capacities of the routes accordingly
storageCapacityFactor - tested for flowCapacityFactor^(0.75)
snapshotperiod
mainMode - modes that use routing in qsim (usually car)
numberOfThreads - for parallel QSim
```

### Network

```
inputNetworkFile - path to MATSim network
```

### Plans

```
inputPlansFile - path to plans/population file
```

### Transit

```
useTransit - specify whether PT should be simulated
transitScheduleFile - path to transit schedule file
vehiclesFile - path to vehicles file
routingAlgorithmType - the type of transit routing algorithm used (default: SwissRailRaptor)
transitModes - for public transit equals to "pt"
```

### Facilities

```
todo
```

### Households

```
todo
```
