# Activity Chains

To pair each synthetic agent with activities we first extract the daily activity chains from the travel survey.

#### Activity Chain

A series of activities that each agent performs during one day. Our tracked activities so far are:

* Primary:
  * home
  * work
  * education
* Secondary (TBA):
  * leisure
  * shop
  * other

Activity chain consists of activities and trips between each two activities:

Activity: \* type \* start time \* end time \* activity order \* location ID \[optional]

Trip: \* traveling mode \* trip order

For simulation purposes first activity type must be the same as the last activity type. For example, if synthetic agents starts the day performing a "home" activity, they must also end the day performing a "home" activity, such that the activity chain is looped and can, in theory, run indefinitely.

## Extracting Activity Chains from Travel Survey

We extract looped activity chain for each traveler in our travel survey data. Travelers with invalid activity chains are discarded.&#x20;

## Matching census data and travel survey

To assign each synthetic agent an activity chain, we use statistical matching to map traveler onto our census data.

Attributes used for Hot Deck matching algorithm:

* age: grouped into categories {0-17,18-29,30-44,45-64,65-74,75+}
* employment status: grouped into categories {employed, unemployed, student}
* sex: two groups {male, female}
* district \[optional]

Roughly 1000 samples from our census data remained unmatched with a counterpart from the travel survey. The unmatched agents were mostly 30+ aged students. We dropped those samples from our dataset.
