---
description: >-
  This page describes the current state of affairs with SynthPop and MATSim
  simulation.
cover: >-
  https://images.unsplash.com/photo-1497005367839-6e852de72767?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw2fHxwcm9ncmVzc3xlbnwwfHx8fDE2NDg0ODI5NDg&ixlib=rb-1.2.1&q=85
coverY: 0
---

# Current state

### **Repositories**

#### **Metacity-SynthPop:**

Population with travel demands - primary and secondary.

newest generated population file: 4/11/2022

Possible future improvement:

* students assigned to schools by age group
* location assignment should be also mode dependent
* missing travels: people from Prague metropolitan region, travels partly outside Prague, tourism, logistics

#### **Metacity-MATsimOutput:**

MATsim output exporter - converts MATSim output files to internal SIM format. Functional export for each vehicle type, with an updated set of passengers. Person trips (individual daily travels as SIM) are currently not implemented.

### **Metacity-MATSim**

* contains working configuration for MATSim

But: Last simulation run in Tramola under Secondary locations scenario - 500 iterations, run 23.

Modes: car, pt, walk, bike, ride

Network: OSM + connected cycleways for bike transport

#### Key input files:

### Tramola environment

Available at: A/META/matsim/tramola-21.0.6/

Run with: java -Xmx2g -jar tramola-21.0.6.jar
