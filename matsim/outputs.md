# Output Files

Please refer to: [https://www.simunto.com/matsim/tutorials/eifer2019/slides_day3.pdf] and [http://ci.matsim.org:8080/job/MATSim-Book/ws/partOne-latest.pdf] p. 15

MATSim basic info google doc:[https://app.gitbook.com/o/-MfgvryXuleE6_RAFPwT/s/-MfbtA6qb2wv3imywpb-/matsim/basic-info]

Files continuously updated during the run of simulation:
- Log
- Warnings and errors log
- Score statistics - score of agents plans
- Leg travel distance statistics
- Stopwatch - computer time of actions like replanning or execution of the simulation

Files created after an iteration:
- Events (output_events.xml.gz)
- Plans (output_plans.xml.gz)
- Leg histogram
- Trip durations - e.g. average time travel by mode
- Link stats - e.g. link volumes (counted LinkLeave-Events)

## Plans vs. events
Plan = input for the simulation (how agents plan to move for the iteration)
Event = output of the simulation, plan is realized by a series of events (~ executed plans)

Events and Plans often do not match. Assumptions to create plans might be wrong. Interaction with other agents cannot be foreseen.



### Output_events.xml.gz
Is the only real output of MATSim.
The XML file contains every action, executed either by an agent, vehicle or pther entity, in the simulation recorded as an event. 

Each event has (guaranteed):
- time (timestamp)
- type (actstart, actend - agent’s activities; departure, arrival - agent’s leg mode; enter link, leave link - vehicle’s event; ...) 
- actor in the form of person_id, vehicle_id etc..
+ additional number of key-value pairs (vehicle id, link id, ...).

The file should be interpreted as a stream of data, which can be further analysed. Stream is sorted by event time (seconds). In many cases, events appear in pairs (start/end activity, depart/arrive, enter/leave link, ...).


### Output_plans.xml.gz
A file containing the list of agents and their final plan. 

MATSim module “PrepareForSim” calculates the (these are added in the file):
- route for each agent's trip
- plan’s score
- whether one of the plans was selected or not


### Output_experienced_plans.xml.gz
Plans with calculated travel time (the “real” time it took to travel - instead of planned). Contains information about executed plan of each agent in simulation.

### Output_trips.csv.gz
A file listing all planned trips of each agent. 
Similar to a travel diary(?)
Properties:
person id; trip_number; trip_id; dep_time; trav_time; wait_time; traveled_distance; euclidean_distance; main_mode; longest_distance_mode; modes; start_activity_type; end_activity_type; start_facility_id; start_link; start_x; start_y; end_facility_id; end_link; end_x; end_y; first_pt_boarding_stop; last_pt_egress_stop

### Output_vehicles.xml.gz
Defined vehicles in MATSim (defined type, capacity, … and list of each vehicle).

### Output_transitVehicles.xml.gz
Similar to output_vehicles.xml.gz but for public transport (defined by GTFS, see Testing Data - Public Transport)[https://app.gitbook.com/o/-MfgvryXuleE6_RAFPwT/s/-MfbtA6qb2wv3imywpb-/matsim/basic-info].

### Output_allVehicles.xml.gz
Similar to output_vehicles.xml.gz and output_transitVehicles.xml.gz but contains every vehicle defined for the simulation.

### Output_persons.csv.gz
List of persons (agents), their score and starting activity. 
Properties
person; executed_score; first_act_x; first_act_y; first_act_type


### Output_counts.xml.gz
MATSim can compare the simulated traffic volumes to traffic counts from the real world.



### Output_network.xml.gz
Describes MATSim network. A list of links and nodes.
Each node has coordinate point. Each link connects two nodes.


### Output_transitSchedule.xml.gz
### Output_facilities.xml.gz
### Output_households.xml.gz
### Output_legs.csv.gz

