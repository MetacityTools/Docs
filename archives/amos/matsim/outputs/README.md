# Output

Please refer to: \[https://www.simunto.com/matsim/tutorials/eifer2019/slides\_day3.pdf] and \[http://ci.matsim.org:8080/job/MATSim-Book/ws/partOne-latest.pdf] p. 15

MATSim basic info google doc:\[https://app.gitbook.com/o/-MfgvryXuleE6\_RAFPwT/s/-MfbtA6qb2wv3imywpb-/matsim/basic-info]

Files continuously updated during the run of simulation:

* Log
* Warnings and errors log
* Score statistics - score of agents plans
* Leg travel distance statistics
* Stopwatch - computer time of actions like replanning or execution of the simulation

Files created after an iteration:

* Events (output\_events.xml.gz)
* Plans (output\_plans.xml.gz)
* Leg histogram
* Trip durations - e.g. average time travel by mode
* Link stats - e.g. link volumes (counted LinkLeave-Events)

## Plans vs. events

Plan = input for the simulation (how agents plan to move for the iteration) Event = output of the simulation, plan is realized by a series of events (\~ executed plans)

Events and Plans often do not match. Assumptions to create plans might be wrong. Interaction with other agents cannot be foreseen.

### Output\_events.xml.gz

Is the only real output of MATSim. The XML file contains every action, executed either by an agent, vehicle or pther entity, in the simulation recorded as an event.

Each event has (guaranteed):

* time (timestamp)
* type (actstart, actend - agent’s activities; departure, arrival - agent’s leg mode; enter link, leave link - vehicle’s event; ...)
* actor in the form of person\_id, vehicle\_id etc..
* additional number of key-value pairs (vehicle id, link id, ...).

The file should be interpreted as a stream of data, which can be further analysed. Stream is sorted by event time (seconds). In many cases, events appear in pairs (start/end activity, depart/arrive, enter/leave link, ...).

### Output\_plans.xml.gz

A file containing the list of agents and their final plan.

MATSim module “PrepareForSim” calculates the (these are added in the file):

* route for each agent's trip
* plan’s score
* whether one of the plans was selected or not

### Output\_experienced\_plans.xml.gz

Plans with calculated travel time (the “real” time it took to travel - instead of planned). Contains information about executed plan of each agent in simulation.

### Output\_trips.csv.gz

A file listing all planned trips of each agent. Similar to a travel diary(?) Properties: person id; trip\_number; trip\_id; dep\_time; trav\_time; wait\_time; traveled\_distance; euclidean\_distance; main\_mode; longest\_distance\_mode; modes; start\_activity\_type; end\_activity\_type; start\_facility\_id; start\_link; start\_x; start\_y; end\_facility\_id; end\_link; end\_x; end\_y; first\_pt\_boarding\_stop; last\_pt\_egress\_stop

### Output\_vehicles.xml.gz

Defined vehicles in MATSim (defined type, capacity, … and list of each vehicle).

### Output\_transitVehicles.xml.gz

Similar to output\_vehicles.xml.gz but for public transport (defined by GTFS, see Testing Data - Public Transport)\[https://app.gitbook.com/o/-MfgvryXuleE6\_RAFPwT/s/-MfbtA6qb2wv3imywpb-/matsim/basic-info].

### Output\_allVehicles.xml.gz

Similar to output\_vehicles.xml.gz and output\_transitVehicles.xml.gz but contains every vehicle defined for the simulation.

### Output\_persons.csv.gz

List of persons (agents), their score and starting activity. Properties person; executed\_score; first\_act\_x; first\_act\_y; first\_act\_type

### Output\_counts.xml.gz

MATSim can compare the simulated traffic volumes to traffic counts from the real world.

### Output\_network.xml.gz

Describes MATSim network. A list of links and nodes. Each node has coordinate point. Each link connects two nodes.

### Output\_transitSchedule.xml.gz

### Output\_facilities.xml.gz

### Output\_households.xml.gz

### Output\_legs.csv.gz
