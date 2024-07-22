## Internship organization background

Satelligence (S11) is a company founded in 2016 that specializes in providing satellite-based actionable information by monitoring environmental risks in commodity supply chains and financial investment planning [@satelligence_home_nodate]. More specifically, the company processes terabytes of satellite imagery to detect environmental risks and presents this information to their clients in a web application to assist them in the migration towards more sustainable sourcing models and the compliance with deforestation-free commodities regulations, such as the European Union Deforestation Regulation (EUDR) [@satelligence_internship_2023]. S11's main focus is continuous deforestation monitoring (CDM) in the tropics using freely accessible satellite imagery. This is a data-intensive task that is achieved by leveraging the benefits of cloud computing, specifically Google Cloud Platform.

## Context and justification of research

Satelligence strongly relies on cloud computing for their services. They process extensive volumes of satellite imagery amounting to terabytes using DPROF, a distributed processing framework created within the company to efficiently process multidimensional spatial datasets. While this processing workflow currently runs smoothly, the company’s data and operations teams face challenges when going deeper into the analysis and accessing intermediate results due to the big nature of this data [@satelligence_internship_2023]. Scholars have defined big data as datasets characterized by their high Volume, Velocity, and Variety, which makes it paramount to use advanced processing and analytics techniques to derive relevant insights [@giri_big_2014]. In the specific case of Satelligence, their datasets can be categorized as big data due to their: High volume (Terabytes of satellite images processed every day), high velocity (Near – real time processing of these images) and high variety (Imagery coming from different sensors and regions). All these datasets are a specific case of big data: Big Geodata.

### Significance of the topic and previous research

In the past decades there has been a rapid increase in the amount and size of geo-spatial information that can be accessed. Nowadays, more than 150 satellites orbit the earth collecting thousands of images every single day [@zhao_scalable_2021]. This has made data handling and the introduction of spatial data infrastructures (SDIs) paramount when working with such big datasets.

Traditionally, SDIs have served to ease the accessibility, integration and analysis of spatial data [@rajabifard_spatial_2001]. However, in practice SDIs have been built upon technologies that focus on data preservation rather than accessibility [@durbha_advances_2023]. Due to this, an important shift is underway towards more cloud-based SDIs [@tripathi_cloud_2020]. These platforms need the emergence of new technologies that prioritize seamless access to cloud-stored data, efficient discovery services that ensure the easy location of extensive spatial data, and data visualization interfaces where multiple datasets can be depicted.

#### Cloud-based data storage {.unnumbered}

Spatial data, just like any other type of data, can be cataloged into structured and unstructured data. Structured datasets are often organized and follow a specific structure (i.e. A traditional table with rows (objects) and columns (features)). On the other hand, unstructured data does not have a predefined structure (e.g. Satellite imagery and Time series data) [@mishra_structured_2017]. The management of structured data has witnessed substantial advancements, making it straightforward to handle it systematically using, for instance, relational databases (i.e. With the help of Structured Query Language (SQL)) [@kaufmann_database_2023]. In contrast, due to the additional challenges associated with the handling of unstructured data, the developments in this area have taken a longer time to appear.

The emergence of cloud-based archives has been one of the main advancements for unstructured data management during the last decades. In the specific case of geo-spatial data, it has allowed to store terabytes of unstructured data (i.e. Satellite imagery) on the cloud and access it through the network. However, the necessity transmitting data across networks to access it makes it essential to develop new data formats suited for such purposes [@durbha_advances_2023].

At S11, the storage of large geo-spatial data is already managed using Google Storage Buckets, and they are currently in the process of incorporating the conversion to cloud-optimized data formats like Cloud Optimized GeoTIFFs (COGs) and Zarrs in their processing framework (DPROF) to improve efficiency and accessibility.

**Cloud-optimized data formats** 

*COG* 

Cloud-Optimized GeoTIFFs ([COGs](https://www.cogeo.org/)) are an example of data formats that have been created to ease the access of data stored in the cloud. They improve the readability by including the metadata in the initial bytes of the file stored, storing different image overviews for different scales and tiling the images in smaller blocks. These characteristics make COG files heavier than traditional image formats. However, they also greatly enhance accessibility by enabling the selective transfer of only the necessary tiles using HTTP GET requests [@desruisseaux_ogc_2021]. Additionally, this data format has been adopted as an Open Geospatial Consortium (OGC) standard. These standards are a set of guidelines and specifications created to facilitat data interoperability [@ogc_ogc_2023].

*Zarr*

Another cloud native data format that has gained popularity recently is [Zarr](https://zarr.readthedocs.io/en/stable/). This data format and python library focuses on the cloud-optimization of n-dimensional arrays. Zarr, differently than COGs store the metadata separately from the data chunks using lightweight external JSON files [@durbha_advances_2023]. Additionally, this data format stores the N-dimensional arrays in smaller chunks that can be accessed more easily. While the storage of Zarr files in chunks facilitates more efficient data access, the absence of overviews hinders the visualization of this data in a web map service [@desruisseaux_ogc_2021]. Due to the increasing use of Zarr for geo-spatial purposes, the OGC endorsed Zarr V2 as a community standard. Nevertheless, efforts are still being made to have a geo-spatial Zarr standard adopted by OGC [@chester_ogc_2024].

#### Data discovery services {.unnumbered}

A discovery service that recently has become widely used for the exploration of big geo-data is Spatio-Temporal Asset Catalog (STAC). Through the standardization of spatio-temporal metadata, STAC simplifies the management and discovery of big geo-data [@brodeur_geographic_2019]. This service works by organizing the data into catalogs, collections, items, and assets stored as lightweight JSON formats (See @tbl-stac-comps) [@durbha_advances_2023].

Moreover, there are two types of STAC catalogs: static and dynamic. Static catalogs are pre-generated and stored as static JSON files on a cloud storage. Static catalogs follow sensible hierarchical relationships between STAC components and this feature makes it easy to be browsed and/or crawled by. Nevertheless, these catalogs cannot be queried. On the other hand, dynamic catalogs are generated as APIs that respond to queries dynamically. Notably, dynamic catalogs will show different views of the same catalog depending on queries which usually focus on the spatio-temporal aspect of the data [@noauthor_stac-specbest-practicesmd_nodate].

| **STAC components** | **Description**                                                                                          |
|----------------------------------------|--------------------------------|
| *Assets*            | An asset can be any type of data with a spatial and a temporal component.                                |
| *Items*             | An item is a GeoJSON feature with some specifications like: Time, Link to the asset (e.g. Google bucket) |
| *Collections*       | Defines a set of common fields to describe a group of Items that share properties and metadata           |
| *Catalogs*          | Contains a list of STAC collections, items or can also contain child catalogs.                           |

: STAC components {#tbl-stac-comps .striped .hover}

In the specific case of dynamic catalogs, the concept of [STAC API](https://github.com/radiantearth/stac-api-spec/) is widely used. In general, an API is a set of rules and protocols that enables different software applications to communicate with each other. In the case of the STAC API, it provides endpoints for searching and retrieving geospatial data based on criteria such as location and time, delivering results in a standardized format that ensures compatibility with various tools and services in the geospatial community. Moreover, even though STAC API is not an OGC standard or a OGC community standard, the basic requests performed in a STAC API adheres to the [OGC API-Features](https://ogcapi.ogc.org/features/) standards for querying by bounding box and time range, returning GeoJSON-formatted results that conform to both STAC and OGC specifications. Ultimately, compared to [OGC API-Features](https://ogcapi.ogc.org/features/), [STAC API](https://github.com/radiantearth/stac-api-spec/) enhances functionality by providing additional features that users needed (e.g. cross-collection search, versioning).

#### Visualization interfaces {.unnumbered}

The visualization of spatial data brings with it a series of challenges due to its big nature. Dynamic tiling libraries such as [TiTiler](https://developmentseed.org/titiler/) have tackled multiple of these challenges by creating APIs that dynamically generate PNG/JPEG image tiles when requested without reading the entire source file into memory [@noauthor_titiler_nodate]. This feature optimizes rendering of images since PNG and JPEG image file formats are more easily transferred through the web.

TiTiler supports various data structures including STAC (SpatioTemporal Asset Catalog), Cloud Optimized GeoTIFFs (COGs), and is currently working on adding support for Zarrs. For the first two the [TiTiler PgSTAC](https://github.com/stac-utils/titiler-pgstac) specialized extension integrates with PostgreSQL to enhance STAC catalog querying capabilities. For the case of Zarrs, the [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) extension is being developed to facilitate the handling of multidimensional data arrays.

### Added value of this research

This research aims to identify efficient solutions for the company's current challenges in discovering and visualizing large geospatial datasets by integrating cloud-optimized data formats, cloud services, STAC specifications, and dynamic tiling services. The outcomes of this research will: offer valuable insights into the existing data discovery challenges within the company, propose a methodology for integrating discovery and visualization services, and evaluate the effectiveness of dynamic tiling for various cloud-optimized data formats.

## Research questions

-   What are the current challenges, practices, and user experiences related to data discovery and data visualization in the company?
-   How does the integration of cloud-optimized data formats, cloud services and SpatioTemporal Asset Catalog (STAC) specifications influence the process and experiences of discovering big spatial data?
-   To what extent do dynamic tiling services can perform in visualizing different cloud-optimized data formats?