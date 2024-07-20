

## Internship organization background

Satelligence is a company founded in 2016 that specializes in providing satellite-based actionable information by monitoring environmental risks in commodity supply chains and financial investment planning [@satelligence_home_nodate]. More specifically, the company processes terabytes of satellite imagery to detect environmental risks and presents this information to their clients in a web application to assist them in the migration towards more sustainable sourcing models and the compliance with deforestation-free regulations, such as the EUDR [@satelligence_internship_2023].

## Context and justification of research

Satelligence currently employs cloud computing, specifically Kubernetes, to process extensive volumes of satellite imagery amounting to terabytes. While this processing workflow currently runs smoothly, the company’s data team faces challenges when going deeper into the analysis and accessing intermediate results due to the big nature of this data [@satelligence_internship_2023]. Scholars have defined big data as datasets characterized by their high Volume, Velocity, and Variety, which makes it paramount to use advanced processing and analytics techniques to derive relevant insights [@giri_big_2014]. In the specific case of Satelligence, their datasets can be categorized as big data due to their: High volume (Terabytes of satellite images processed every day), high velocity (Near – real time processing of these images) and high variety (Imagery coming from different sensors and regions). All these datasets are a specific case of big data: Big Geodata.

### Significance of the topic

In the past decades there has been a rapid increase in the amount and size of geospatial information that can be accessed. Nowadays, more than 150 satellites orbit the earth collecting thousands of images every single day [@zhao_scalable_2021]. This has made data handling and the introduction of spatial data infrastructures (SDIs) paramount when working with such big datasets.

Traditionally, SDIs have served to ease the accessibility, integration and analysis of spatial data [@rajabifard_spatial_2001]. However, in practice SDIs have been built upon technologies that focus on data preservation rather than accessibility [@durbha_advances_2023]. Due to this, an important shift is underway towards more cloud-based SDIs. These platforms need the emergence of new technologies that prioritize seamless access to cloud-stored data, efficient discovery services that ensure the easy location of extensive spatial data, and data visualization interfaces where multiple datasets can be depicted.

#### Cloud-based data storage

Spatial data, just like any other type of data, can be catalogued into structured and unstructured data. Structured datasets are often organized and follow a specific structure (i.e. A traditional table with rows (objects) and columns (features)). On the other hand, unstructured data does not have a predefined structure (e.g. Satellite imagery and Time series data) [@mishra_structured_2017]. The management of structured data has witnessed substantial advancements, making it straightforward to handle it systematically using, for instance, relational databases (i.e. With the help of Structured Query Language (SQL)) [@kaufmann_database_2023]. In contrast, due to the additional challenges associated with the handling of unstructured data, the developments in this area have taken a longer time to appear.

The emergence of cloud-based archives has been one of the main advancements for unstructured data management during the last decades. In the specific case of geo-spatial data, it has allowed to store terabytes of unstructured data (i.e. Satellite imagery) on the cloud and access it through the network. However, the necessity transmitting data across networks to access it makes it essential to develop new data formats suited for such purposes [@durbha_advances_2023].

Cloud-Optimized GeoTIFFs ([COGs](https://www.cogeo.org/)) are an example of data formats that have been created to ease the access of data stored in the cloud. They improve the readability by including the metadata in the initial bytes of the file stored and storing different image tiles for different scales. These characteristics make COGs heavier than traditional image formats, however, they also greatly enhance accessibility by enabling the selective transfer of only the necessary tiles [@durbha_advances_2023].

Another cloud native data format that has gained popularity recently is [Zarr](https://zarr.readthedocs.io/en/stable/). This data format and python library focuses on the cloud-optimization of n-dimensional arrays. Zarr differently than COGs store the metadata separately from the data chunks. Normally, using external JSON files [@durbha_advances_2023].

#### Data discovery services

A discovery service widely used for the exploration of big geodata (e.g. COGs) is Spatio-Temporal Asset Catalog (STAC). Through the standardization of spatial data, STAC simplifies the management and discovery of big geodata [@brodeur_geographic_2019]. This service works by organizing the data into catalogs, collections, items, and assets that will only be read when a computation is required [@durbha_advances_2023]. This feature optimizes workflows by reducing unnecessary data loading.

##### STAC fundamentals

A Catalog built under STAC specifications is composed by:

| **STAC components** | **Description**                                                                                          |
|----------------------------------------|--------------------------------|
| *Assets*            | An asset can be any type of data with a spatial and a temporal component.                                |
| *Items*             | An item is a GeoJSON feature with some specifications like: Time, Link to the asset (e.g. Google bucket) |
| *Collections*       | Defines a set of common fields to describe a group of Items that share properties and metadata           |
| *Catalogs*          | Contains a list of STAC collections, items or can also contain child catalogs.                           |

: STAC components {.striped .hover}

#### Visualization interfaces

The visualization of spatial data brings with it a series of challenges due to its big nature. Dynamic tiling libraries such as TiTiler have tackled multiple of these challenges by dynamically generating PNG/JPEG image tiles only when requested without reading the entire source file into memory [@noauthor_titiler_nodate]. This feature optimizes rendering of images since PNG and JPEG image file formats are more easily transferred through the web.

### Added value of this research

This research aims to develop a STAC catalog to facilitate the discovery of diverse big geospatial datasets created by different teams within the company. Depending on the time available for the thesis, data visualization functionalities may be integrated. Moreover, the research will explore tools to efficiently manage a STAC catalog containing both static and dynamic datasets.

## Research questions

- What are the current challenges, practices, and user experiences related to data discovery and data visualization in the company?
- How does the integration of cloud-optimized data formats, cloud services and SpatioTemporal Asset Catalog (STAC) specifications influence the process and experiences of discovering big spatial data?
- To what extent do dynamic tiling services can perform in visualizing different cloud-optimized data formats?