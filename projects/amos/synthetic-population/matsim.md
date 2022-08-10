# Export of population to MATSim

## Population.xml or Plans.xml (demand)

Eventually each agent in the synthetic population pipeline has the following attributes:
* Personal information: 'person_id', 'trip_today', 'car_avail', 'driving_license'
* Activity information: 'purpose', 'start_time', 'end_time', 'geometry', 'activity_order', 'location_id'
* Travel mode information: 'traveling_mode', 'trip_order'

These attributes are exported to the following format:
```
	<person id="1000367">
		<attributes>
			<attribute name="carAvail" class="java.lang.String">never</attribute>
			<attribute name="hasLicense" class="java.lang.String">no</attribute>
		</attributes>
		<plan selected="yes">
			<activity type="home" x="733199.000629" y="1042609.235624" end_time="07:43:04" ></activity>
			<leg mode="ride"></leg>
			<activity type="education" x="733440.449952" y="1042600.469421" start_time="07:49:47" end_time="12:58:20" ></activity>
			<leg mode="walk"></leg>
			<activity type="home" x="733199.000629" y="1042609.235624" start_time="13:09:56" end_time="15:00:18" ></activity>
			<leg mode="walk"></leg>
			<activity type="leisure" x="733715.3529" y="1043073.3935" start_time="15:05:04" end_time="17:58:23" ></activity>
			<leg mode="walk"></leg>
			<activity type="home" x="733199.000629" y="1042609.235624" start_time="18:03:35" >
			</activity>
		</plan>
	</person>
```

# Comparison notebooks

To compare the results of different synthetic population pipelines and our input data, we use the following notebooks:
[GitHub directory](https://github.com/MetacitySuite/Metacity-SynthPop/blob/master/synthesis/stats)
