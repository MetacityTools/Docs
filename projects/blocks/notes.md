# Block Edition Brainstorm

[MIRO](https://miro.com/app/board/uXjVOXxARPM=/)

6 stages
* create
* interact
* aggregate
* analyze
* insight
* decision


## Data pipeline

Datovy vstup pred abstrakci:
* stavajici zastavba
* pocty rezidentu (?)
* potreby rezidentu (?)
* stavajici komunikace a linky mhd s jizdnimi rady (lepsi v ramci abstrakce zjednodušit na zastávky a kapacity linek/hod )
* segmentace a analyza hoodu z leteckych fotek (CNN:,) ale k čemu?)
* (moznost) zonovani v dané oblasti

Predmet klasifikace a abstrakce:
* domy podle vyuziti: residential, commercial, civic, mix
* plochy podle vyuziti: landmarks, parky, voda, parkoviste, other
* bloky podle velikosti a vyuziti
* residenti v oblasti a jejich potreby - co určuje hustotu lidi v místě a jejich potreby
* bloky podle dosažitelnosti od bodu zájmu (mozna jen interne)
* dopravní obsluha - linky mhd - umisteni, pocet, kapacita/hod, vytíženost linky (metrika?), vytíženost dopravniho uzlu 
* komunikace podle modu dopravy
* komunikace podle intenzity dopravy
* hoody podle bloku - vyuziti
* hoody podle typu residentu (data ??)
* hoody podle toku lidi (data ??) (odliv, příliv)
* hoody podle dalsich metrik (green spaces, noise, dust levels, temperature and so on).

Predmet zmeny inputu:
* domy podle vyuziti: residential, commercial, civic, recreation, mix
* plochy podle vyuziti
* změna plochy v zástavbu a naopak
* residenti v oblasti a jejich potreby (? samostatny faktor nebo vazano na zástavbu ?)
* bloky podle velikosti a vyuziti
* komunikace podle modu dopravy
* komunikace podle intenzity dopravy
* dopravní obsluha - linky mhd - umisteni, kapacita/hod

Predmet zmeny outputu:
* bloky a hoody podle dosažitelnosti (typ dopravy, cas dopravy, vytok, odtok casy)
* poptavka po objektech v blízkosti (co je kritérium?)
* vytíženost linky
* vytíženost komunikace
* vytizenost zastavky
* hoody podle bloku - vyuziti/aktivity (v %)
* hoody podle typu residentu (data ??)
* hoody podle toku lidi (data ??) (odliv, příliv)
* hoody podle dalsich metrik (density, walkability, green spaces, noise, dust levels, temperature, crime and so on).
* network-based: diversity, accessibility

## Visuals
Vizualizace v gridu, 4-okolí:
komunikace a linky: usecky, manhattan
budovy: kvádr


## Other

* generativni rust okoli
* generace bloku podle hood patternu
* kreslení do modelu
* porovnani zonovani s jinými svetovymi mesty



## References
[ CITY SEER BENCHMARK URBANISM ](https://cityseer.benchmarkurbanism.com/)
Secondly, what exactly is it that urbanists should be benchmarking?
[ - Gareth Simons ](https://www.researchgate.net/publication/356377308_Detection_and_prediction_of_urban_archetypes_at_the_pedestrian_scale_computational_toolsets_morphological_metrics_and_machine_learning_methods)