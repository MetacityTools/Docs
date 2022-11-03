# MATSim

## Introduction
Microscopic modeling of traffic: MATSim performs integral microscopic simulation of resulting traffic flows and the congestion they produce.
Microscopic behavioral modeling of demand/agent-based modeling: MATSimuses a microscopic description of demand by tracing the daily schedule and the synthetic travelers’ decisions.  In retrospect, this can be called “agent-based”.
Computational physics: MATSim performs fast microscopic simulations with 10^7 or more “particles”.
Complex adaptive systems/co-evolutionary algorithms: MATSim optimizes the experienced utilities of the whole schedule through the co-evolutionary search for the resulting equilibrium or steady state.

## Simulation stages
Initial demand - describes mobility behaviour (list of agents and their plans)
Execution - “mobsim”, agents and vehicles are moved around in the network 
Scoring - after execution of the plans end, the plans are evaluated based on the execution
Replanning - performed by “strategy modules”
Analysis - at the end of complete simulation, performed automatically or separate post-process


## Terminology
https://www.matsim.org/docs/userguide/terminology 
Agent-Based Transport Simulation

Agent: A synthetic person, part of a synthetic population.
Plan: the intention of an agent, typically for one day."going to work at 07:30, go shopping on the way home at 17:00, be home at 17:45".Each agent needs at least one plan.
Choice set: “plan set” of an agent
Choice set generation: time mutation/re-route/...; innovation of existing plans
Choice: replanning - random replanning strategy
Convergence: learning rate
Score: utility of a plan after it was simulated.
Scenario, Model: several datasets and parameters describing infrastructure (supply) and demand in a region. "Scenario" and "model" are often used without differentiation in MATSim's context.
MATSim Loop
scenario data: description of infrastructure (road network, transit schedule, ...) and agents.
execution: Agents' travel plans are executed in parallel.A.k.a.: mobsim (mobility simulation), synthetic reality, network loading, traffic flow simulation
scoring: each executed plan obtains a score.
replanning: some agents change plans or create new ones.
analyses: traffic volumes, average speeds, utility changes, emissions, accessibility, …
Iterations: execution, scoring and replanning are iteratively performed.
Optimization: each agent tries to optimize its day.
Evolutionary algorithm: each agent has a set of plans ("choice set"), adding new plans(replanning), removing bad ones after a while.
Co-Evolutionary algorithm: If a plan performs good or bad also depends on the plans of all the other agents.
Nash-Equilibrium: Each agent tries to optimize its plan egotistically. (≠ system optimum)

Other 
Plans (input) x events (output)
Legs x Trips
Plan contains “activity” (e. g. working) and “legs” (going to work)
Trips - moving from one node to another with a 0 duration activity

## Inputs and outputs 
Inputs (Plans)
Minimal requirements
Network (network.xml)
Roads (car, public transport, metro)
Can be compressed (xml.gz)
Network change events in a file (for timeVariantNetwork)
Demand (population.xml or plans.xml)
Can be compressed (xml.gz)
Agents and their plans (“human teleport”)
Planning and replanning (a lot of replanning causes fluctuations)
1 or more plans (choice set, gene pool) - 1 is executed and scored
Choose 1 plan, create new one (mutation)
Removing bad plans (survival of the fittest)
Configuration (config.xml) - path to input data, simulation settings - how simulation behaves

Config.xml
One (and only argument) when running MATSim
“Interface” for simple MATSim usage
Default empty config-object
Config config = ConfigUtils.createConfig();
new ConfigWriter(config, ConfigWriter.Verbosity.all).write("defaultConfig.xml");
What needs to be configured
input files locations("scenario data", "data containers")
computational/performance settings(e.g. number of threads)
scoring 
parameters
replanning strategies
replanning strategies' behaviour
analyses/output

Outputs [matsim book, p. 15] (notes in MATSim Outputs)
More detailed document about outputs

(Events) Each event has a time and a type and can contain arbitrary additional key-value pairs (vehicle id, link id, ...)
Stream of events defines the movements of each agent (open for analyses)
E. g. agent starts / ends activity ("actstart", "actend"); agent starts / ends a leg ("departure", "arrival"); vehicle enters / leaves a link ("enter link", "left link")
(Plans) Current state of population (plans) - each iteration, final iteration
Analysis can be made directly with MATSim API (using EventHandlers) - https://github.com/matsim-org/matsim-code-examples/search?q=filename:RunEventsHandling 

Population 
Population = collection of synthetic persons with attributes (home location, age, gender, employment,...)
Demand = activities and trips performed by agents (activity type and location, start and end time, travel mode between activities, travel route)

Best Practice
Create a synthetic population (agents)
Assign demands (plans) to the agents
Or
Use existing population synthesizer
Convert the output to a MATSim population

## Population
Create facilities in zones, randomly distribute or according to data
Create group of persons in the zone with basic demographic attributes
Assign persons into home facilities in the zones

Demand
Travel diaries as agents plans
Make a template from the travel diary
Copy activities from the travel diary to an agent (home, work, shopping)
Assign locations (facilities) to the activities
Randomize activity end times from the template - up-scaling, data are rounded to 10-15 min, < 1% population data


## Simulation 
Deterministic PT Simulation
https://github.com/SchweizerischeBundesbahnen/matsim-sbb-extensions 
Cars and buses are simulated in queue network
PT should be simulated differently (trains in the queue network run fast because there's no stopping them! - arrive too early according to the schedule) - problem: railway PT is too precise (thanks to teleport)
Scoring - utility, fitness
[matsim book, p. 39]
Compares different plans
Negative utility (agents travelling; monetary costs - tolls, fares; arriving late; leaving early)
Positive utility (agent performing activities)

Higher score means a “better” plan (better performance).

Each agent maintains multiple plans for the day, which are scored when the plan is executed (in the mobsim), selected and sometimes modified.
Phases

Mobsim 
The mobility simulation takes one “selected” plan per agent and executes it in a synthetic reality. 

Scoring 
The actual performance of the plan in the synthetic reality is taken to calculate each executed
plan’s score.

Replanning 
PLAN REMOVAL If an agent has more than the maximum number of plans (configurable) then some plans can be discarded (by a plan selector). 
INNOVATION For some agents, a plan is copied, modified, and selected for the next iteration.
CHOICE All other agents choose between their plans.

Can be configured in “planCalcScore” module:

```
<module name="planCalcScore">
<param name="learningRate" value="1.0" />
<param name="BrainExpBeta" value="2.0" /> 	
… 
<parameterset type="modeParams" >         	
<param name="mode" value="car" />                 	
<param name="dailyMonetaryConstant" value="-5.3" /> 	
… 
</module> 
```

## Links
Slides
https://www.simunto.com/matsim/tutorials/eifer2019/ 

Newest lecture
https://www.matsim.org/content/2020-matsim-class-tu-berlin-matsim-version-12x 
Log in as guest to access slides

Full texts
MATSim book: http://ci.matsim.org:8080/job/MATSim-Book/ws/partOne-latest.pdf 
https://www.ubiquitypress.com/site/books/e/10.5334/baw/


