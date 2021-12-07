---
description: MATSim inputs
---

# Inputs

### Network.xml

* Roads (car, public transport, subway)
* Network change events in a file (for timeVariantNetwork)

### Population.xml or Plans.xml (demand)

Agents and their plans&#x20;

* Planning and replanning (a lot of replanning causes fluctuations)
* 1 or more plans (choice set, gene pool) - 1 is executed and scored
* Choose 1 plan, create new one (mutation)
* Removing bad plans (survival of the fittest)

### Config.xml

* One (and only argument) when running MATSim
* “Interface” for simple MATSim usage
* What needs to be configured
  * input files locations("scenario data", "data containers")
  * computational/performance settings(e.g. number of threads)
  * scoring&#x20;
  * parameters
  * replanning strategies
  * replanning strategies' behaviour
  * analyses/output

### Public transport data

Transit schedule of the public transport and how they interact with the network
