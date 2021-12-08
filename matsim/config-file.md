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

### PlanCalcScore

```
writeExperiencedPlans

learningRate
BrainExpBeta
lateArrival
earlyDeparture
performing
marginalUtilityOfMoney
waiting
waitingPt

parameterset type="modeParams" - specifies parameters for each leg mode (e.g. car)
    mode
    constant
    dailyUtilityConstant
    marginalUtilityOfDistance_util_m
    marginalUtilityOfTraveling_util_hr
    monetaryDistanceRate
    dailyMonetaryConstant
    
parameterset type= "activityParams" - must be described for every activity in plans/population file (e.g. home, work, shop, leisure, ...). There are also activities specified by the simulation (e.g. car interaction), do not have to be defined explicitly.
    activityType
    priority
    typicalDuration
    openingTime
    latestStartTime
    earliestEndTime
    closingTime
```

### Planscalcroute

```
networkModes - leg modes that will be reated by the network router (usually cars)

parameterset type="teleportedModeParameters" - leg modes using teleport (walk, ...)
    beelineDistanceFactor
    mode
    teleportedModeSpeed
```

### TimeAllocationMutator

```
mutationAffectsDuration
mutationRange - defines how many seconds a time mutation can maximally shift a time
```

### SwissRailRaptor

```
useCapacityConstraints
```

### Strategy

```
maxAgentPlanMemorySize
fractionOfIterationsToDisableInnovation - disables innovation stage after a % number of iterations

parameterset type="strategysettings" - various strategies for running the simulation
    strategyName - values [BestScore, ChangeExpBeta, ReRoute, SubtourModeChoice, TimeAllocationMutator_ReRoute]. *Some of these need their own module to further specify the usage.
    weight

*module: subtourModeChoice
    chainBasedModes - modes that preserve mass
    modes
    considerCarAvailability
```
