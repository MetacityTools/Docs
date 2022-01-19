# Creating a Synthetic Population
A document describing various workflows to create a synthetic population, which is further used to generate travel demands (plans) for the [MATSim](https://docs.google.com/document/d/1FBWCIPXkKwGI7Jez9SENUg-B-HAw9e81kdxODJoNUVA/edit?usp=sharing) application.

## Introduction
A [synthetic population](https://silo.zone/synPop.html) is a simplified microscopic representation of the actual population. Simplified, because not all attributes are included (for example hair color is deemed to be irrelevant for land use modeling, thus it is ignored). It is microscopic because every household and every person is represented individually. A synthetic population is not identical to the actual population, i.e. most reside
nts will find a record in the synthetic population that matches themselves. Instead, the synthetic population matches various statistical distributions of the actual population, and therefore, is close enough to the true population to be used in modeling
- More information: [Creating a Synthetic Population, 2003 ](http://moeckel.github.io/rm/doc/2003_moeckel_etal_synpop_cupum.pdf)
- (outdated) Possible input [data requirements](https://docs.google.com/document/d/1xIxFxnPrZ9Fja_9PfYYTN6YLcxZzfu4d14OFNExxBMU/edit?usp=sharing) for generating synthetic population of Prague and Central Bohemian Region. 
- Comprehensive tutorial, something of a MasterDoc: [Generating Synthetic Populations for Social Modeling, Sao Paulo, 2017](https://nssac.bii.virginia.edu/~swarup/synthetic_population_tutorial_2/AAMAS_2017_generating_synthetic_populations_for_social_modeling_full_tutorial.pdf) and earlier version [Generating Synthetic Populations for Social Modeling, Singapore, 2016](https://biocomplexity.virginia.edu/sites/default/files/staff/AAMAS_2016_generating_synthetic_populations_for_social_modeling_full_tutorial.pdf)
- SynthPop for MATSim: [Generation of a Synthetic Population for MATSim Models, Master's Thesis, 2017](https://diglib.tugraz.at/download.php?id=5aa247f495fb1&location=browse)

#### Simulation Note
 MATSim is an activity-based model, which requires more detailed information on household demographics. **The collection of individual micro data (data that can be associated with single buildings, individuals)  is  neither  allowed nor desirable  for  privacy reasons.**

A **model** is needed to generate **synthetic micro data** from general accessible **aggregated data**.

## Synthetic Population
 todo: image
 * data and attributes of a modeled agent are synthesized by integrating a diverse set of data sources and using models for interpolation and extrapolation of data.
* Syntetic Agent is *similar* to real individuals but is not identical to any individual in the population.
* Individual attributes of agents and their links are based on real-world collected data.
* Privacy of individuals is protected.
* The correlations between the data sets agree with the measured correlations of data in the real world.

### Synthetic Agent
A *representation* of elements’ states and interactions that is *not intended to match any snapshot of the system*, but to provide a statistically accurate overall picture. [AAMAS16]
### Synthetic Population
A synthetic population represents a set of synthetic agents that share a common geographic, social or biological characteristic (e.g.
people in an rural or urban region, individuals from a given tribe, etc) A
synthetic society is a synthetic population that is endowed with other
social and behavioral attributes that allow the individuals within a
population to interact by following these social, behavioral norms and
rules.[AAMAS16]

### Synthetic Network
A synthetic network is composed of synthetic agents combined with links that capture interactions among the agents.[AAMAS16]


## Motivation 
Although microdata are commonly made publicly available as census samples [8] or household
survey samples [9], full census microdata are almost never publicly released to protect the privacy of respondents. A more realistic option for researchers to obtain a dataset of all household observations and associated characteristics in a population is to simulate it, and recent advances in generating synthetic populations have made this approach a viable alternative [10]. 
Well generated synthetic population may also represent the key attributes of the real population ina generalized state, leading to lower data complexity.
Synthetic population datasets also have the advantage over actual census data that multiple scenarios can be generated to test outcomes in potential future populations.

