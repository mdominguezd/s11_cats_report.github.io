## Internship organization background

Satelligence (S11) is a company founded in 2016 that specializes in providing satellite-based actionable information by monitoring environmental risks in commodity supply chains and financial investment planning [@satelligence_home_nodate]. More specifically, the company processes terabytes of satellite imagery to detect environmental risks and presents this information to their clients in a web application to assist them in the migration towards more sustainable sourcing models and the compliance with deforestation-free commodities regulations, such as the European Union Deforestation Regulation (EUDR) [@satelligence_internship_2023]. S11's main focus is deforestation monitoring in the tropics using freely accessible satellite imagery. This is a data-intensive task that is achieved by leveraging the benefits of cloud computing, specifically Google Cloud Platform.

## Context and justification of research

Satelligence strongly relies on cloud computing for their services. They process extensive volumes of satellite imagery amounting to terabytes using DPROF, a distributed processing framework created within the company to efficiently process multidimensional spatial datasets. While this processing workflow currently runs smoothly, the company’s data and operations teams face challenges when going deeper into the analysis and accessing intermediate results due to the big nature of this data [@satelligence_internship_2023]. Scholars have defined big data as datasets characterized by their high Volume, Velocity, and Variety, which makes it paramount to use advanced processing and analytics techniques to derive relevant insights [@giri_big_2014]. In the specific case of Satelligence, their datasets can be categorized as big data due to their: High volume (Terabytes of satellite images processed every day), high velocity (Near – real time processing of these images) and high variety (Imagery coming from different sensors and regions). All these datasets are a specific case of big data: Big Geodata.

### Significance of the topic and previous research

In the past decades there has been a rapid increase in the amount and size of geo-spatial information that can be accessed. Nowadays, more than 150 satellites orbit the earth collecting thousands of images every single day [@zhao_scalable_2021]. This has made data handling and the introduction of spatial data infrastructures (SDIs) paramount when working with such big datasets.

Traditionally, SDIs have served to ease the accessibility, integration and analysis of spatial data [@rajabifard_spatial_2001]. However, in practice SDIs have been built upon technologies that focus on data preservation rather than accessibility [@durbha_advances_2023]. Due to this, an important shift is underway towards more cloud-based SDIs [@tripathi_cloud_2020]. These platforms need the emergence of new technologies that prioritize seamless access to cloud-stored data, efficient discovery services that ensure the easy location of extensive spatial data, and data visualization interfaces where multiple datasets can be depicted.

#### Cloud-based data storage {-}

Spatial data, just like any other type of data, can be cataloged into structured and unstructured data. Structured datasets are often organized and follow a specific structure (i.e. A traditional table with rows (objects) and columns (features)). On the other hand, unstructured data does not have a predefined structure (e.g. Satellite imagery and Time series data) [@mishra_structured_2017]. The management of structured data has witnessed substantial advancements, making it straightforward to handle it systematically using, for instance, relational databases (i.e. With the help of Structured Query Language (SQL)) [@kaufmann_database_2023]. In contrast, due to the additional challenges associated with the handling of unstructured data, the developments in this area have taken a longer time to appear.

The emergence of cloud-based archives has been one of the main advancements for unstructured data management during the last decades. In the specific case of geo-spatial data, it has allowed to store terabytes of unstructured data (i.e. Satellite imagery) on the cloud and access it through the network. However, the necessity transmitting data across networks to access it makes it essential to develop new data formats suited for such purposes [@durbha_advances_2023].

At S11, the storage of large geo-spatial data is already managed using Google Storage Buckets, and they are currently in the process of incorporating the conversion to cloud-optimized data formats like Cloud Optimized GeoTIFFs (COGs) and ZARRs in their processing framework (DPROF) to improve efficiency and accessibility.

##### Cloud-optimized data formats {-}

###### COG {-}

Cloud-Optimized GeoTIFFs ([COGs](https://www.cogeo.org/)) are an example of data formats that have been created to ease the access of data stored in the cloud. They improve the readability by including the metadata in the initial bytes of the file stored, storing different image overviews for different scales and tiling the images in smaller blocks. These characteristics make COG files heavier than traditional image formats, however, they also greatly enhance accessibility by enabling the selective transfer of only the necessary tiles using HTTP GET requests [@desruisseaux_ogc_2021].

###### ZARR {-}

Another cloud native data format that has gained popularity recently is [Zarr](https://zarr.readthedocs.io/en/stable/). This data format and python library focuses on the cloud-optimization of n-dimensional arrays. Zarr differently than COGs store the metadata separately from the data chunks using lightweight external JSON files [@durbha_advances_2023]. Additionally, this data format stores the N-dimensional arrays in smaller chunks that can be accessed more easily. Finally, while the storage of ZARR files in chunks facilitates more efficient data access, the absence of overviews hinders the visualization of this data in a web map service [@desruisseaux_ogc_2021].

#### Data discovery services {-}

\hl{AMPLIAR. Maybe include other data discovery services? }

A discovery service widely used for the exploration of big geo-data is Spatio-Temporal Asset Catalog (STAC). Through the standardization of spatial data, STAC simplifies the management and discovery of big geo-data [@brodeur_geographic_2019]. This service works by organizing the data into catalogs, collections, items, and assets that will only be read when a computation is required [@durbha_advances_2023]. This feature optimizes workflows by reducing unnecessary data loading.

##### STAC fundamentals {-}

A Catalog built under STAC specifications is composed by:

| **STAC components** | **Description**                                                                                          |
|----------------------------------------|--------------------------------|
| *Assets*            | An asset can be any type of data with a spatial and a temporal component.                                |
| *Items*             | An item is a GeoJSON feature with some specifications like: Time, Link to the asset (e.g. Google bucket) |
| *Collections*       | Defines a set of common fields to describe a group of Items that share properties and metadata           |
| *Catalogs*          | Contains a list of STAC collections, items or can also contain child catalogs.                           |

: STAC components {.striped .hover}

\hl{Talk about the integration with sturctured data base management (PgSTAC)}

#### Visualization interfaces {-}

\hl{Mention also that there are multiple TiTiler services}

The visualization of spatial data brings with it a series of challenges due to its big nature. Dynamic tiling libraries such as TiTiler have tackled multiple of these challenges by dynamically generating PNG/JPEG image tiles only when requested without reading the entire source file into memory [@noauthor_titiler_nodate]. This feature optimizes rendering of images since PNG and JPEG image file formats are more easily transferred through the web.

### Added value of this research

\hl{expand}

This research aims to develop a STAC catalog to facilitate the discovery of diverse big geo-spatial datasets created by different teams within the company. Depending on the time available for the thesis, data visualization functionalities may be integrated. Moreover, the research will explore tools to efficiently manage a STAC catalog containing both static and dynamic datasets.

## Research questions

-   What are the current challenges, practices, and user experiences related to data discovery and data visualization in the company?
-   How does the integration of cloud-optimized data formats, cloud services and SpatioTemporal Asset Catalog (STAC) specifications influence the process and experiences of discovering big spatial data?
-   To what extent do dynamic tiling services can perform in visualizing different cloud-optimized data formats?