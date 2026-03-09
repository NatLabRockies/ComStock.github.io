---
layout: default
title: Building Type Crosswalks
parent: Resources
nav_order: 1
---

# Building Type Crosswalks

ComStock uses 14 building types to segment the part of the commercial building stock we represent. These building type are: full service restaurant, hospital, large hotel, large office, medium office, outpatient, quick service restaurant, primary school, retail, secondary school, small hotel, small office, strip mall, and warehouse. In some cases, most notably within the strip mall building type, we also model building subtypes. In the case of strip malls the subtypes vary the percent of the building floor area that is used for restaurants, which have significantly different energy use patterns and drastically impact the energy use of a strip mall.

Several tutorials and how to guides discuss matching the building type of an external data source to the ComStock building type to allow ComStock to complement or extend that external data source. One of the challenges, however, is that the ComStock building types (and subtypes) don't always align with external data sources. As discussed at some length in our documentation ComStock models the significant majority, but not all, of the commercial buildings in the U.S. There are many building types we do not currently model, for example laboratories, data centers, movie Theaters, and ice rinks. Even within building types we do model there can be questions - how do I know if "healthcare" should be associated with a hospital or an outpatient facility?

There is no clean, precise answer to this question, but we believe the following will be a useful set of resources for consideration. First of all, it is very useful to read section **3.1.2 Building Type Assignments** in our [reference technical report](https://www.nlr.gov/docs/fy23osti/83819.pdf), particularly the descriptions of each ComStock building type. Second, we highly recommend using google maps, and particularly google street view. Making a practice out of double checking and developing an intuitive understanding of how different external data source think about building types is incredibly helpful. Whenever integrating a new data source we do this ourselves, extensively. Finally, we have provided below the crosswalk used to map building types between different data sources in the current release of ComStock. Ideally this table will serve as a starting point for developing a (hopefully simpler) mapping between your external data source and our building types. This table is also available in a [CSV format here.][1]

[1]:../../../assets/files/commercial_type_crosswalk.csv

| **CoStar Building Type**                | **HIFLD Table**            | **DOE Prototype and ComStock Building Type** | **CBECS Principle Building Activity Plus** |
|-----------------------------------------|----------------------------|----------------------------------------------|--------------------------------------------|
| Retail:   Bar                           | Not applicable             | Full service restaurant                      | Restaurant/cafeteria                       |
| Retail: Restaurant                      | Not applicable             | Full service restaurant                      | Bar/pub/lounge                             |
| Not applicable                          | Healthcare: Hospitals      | Hospital                                     | Hospital/inpatient health                  |
| Hospitality: Hotel                      | Not applicable             | Large hotel                                  | Hotel                                      |
| Hospitality: Hotel casino               | Not applicable             | Large hotel                                  | Hotel                                      |
| Office: Industrial live/work   unit     | Not applicable             | Office                                       | Administrative/professional office         |
| Office: Office live/work unit           | Not applicable             | Office                                       | Bank/other financial                       |
| Office: Office/residential              | Not applicable             | Office                                       | Government office                          |
| Retail: Bank                            | Not applicable             | Office                                       | Medical office (non-diagnostic)            |
| Flex                                    | Not applicable             | Office                                       | Other office                               |
| Office: Service                         | Not applicable             | Office                                       | Other office                               |
| Health care: Rehabilitation   center    | Not applicable             | Outpatient                                   | Medical office (diagnostic)                |
| Health care: Skilled nursing   facility | Not applicable             | Outpatient                                   | Medical office (diagnostic)                |
| Office: Medical                         | Not applicable             | Outpatient                                   | Clinic/other outpatient health             |
| Health care                             | Not applicable             | Outpatient                                   | Clinic/other outpatient health             |
| Not applicable                          | Education: Public schools  | Primary/secondary school                     | Elementary/middle school                   |
| Not applicable                          | Education: Private schools | Primary/secondary school                     | High school                                |
| Retail: Fast food                       | Not applicable             | Quick service restaurant                     | Fast food                                  |
| General retail: Fast rood               | Not applicable             | Quick service restaurant                     | Fast food                                  |
| Retail: Department store                | Not applicable             | Retail                                       | Retail store                               |
| Retail: Freestanding                    | Not applicable             | Retail                                       | Retail store                               |
| Retail: Garden center                   | Not applicable             | Retail                                       | Other retail                               |
| General retail: Freestanding            | Not applicable             | Retail                                       | Other retail                               |
| Hospitality: Motel                      | Not applicable             | Small hotel                                  | Motel or inn                               |
| Hospitality                             | Not applicable             | Small hotel                                  | Motel or inn                               |
| Flex: Showroom                          | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Retail: Storefront                      | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Retail: Storefront   retail/office      | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Retail: Storefront   retail/residential | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Specialty: Post office                  | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Retail                                  | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| General retail                          | Not applicable             | Strip mall                                   | Strip shopping mall                        |
| Flex: Light distribution                | Not applicable             | Warehouse                                    | Distribution/shipping center               |
| Flex: Light manufacturing               | Not applicable             | Warehouse                                    | Distribution/shipping center               |
| Industrial: Distribution                | Not applicable             | Warehouse                                    | Distribution/shipping center               |
| Industrial: Service                     | Not applicable             | Warehouse                                    | Nonrefrigerated warehouse                  |
| Industrial: Showroom                    | Not applicable             | Warehouse                                    | Nonrefrigerated warehouse                  |
| Industrial: Truck terminal              | Not applicable             | Warehouse                                    | Nonrefrigerated warehouse                  |
| Industrial: Warehouse                   | Not applicable             | Warehouse                                    | Self-storage                               |
| Specialty: Airplane hangar              | Not applicable             | Warehouse                                    | Self-storage                               |
| Specialty: Self-storage                 | Not applicable             | Warehouse                                    | Self-storage                               |
