To answer the research questions presented a series of tasks were undertaken. These tasks are presented in the following subsections where they are divided by research question.

## Baseline scenario {#sec-baseline}

The baseline scenario was defined as the set of methods currently being used by members of different teams at Satelligence to find, retrieve and visualize spatial data. This baseline scenario was evaluated qualitatively by interviewing four members of two different teams in the company (i.e. the data and the operations team). To keep a balance regarding experience of the study subjects, both the newest member of each team and a member with at least three years in the company were interviewed.

The questions asked during the interviews were oriented towards two main topics that were covered during this internship: Spatial data discovery and spatial data visualization. For both topics, the questions were divided into questions related to raster and vector datasets. The questions included in the interview can be found in @sec-baseline-q and were meant to be open questions with multiple possible answers.

Furthermore, based on the answers of the interviewees a flowchart was built to represent visually the traditional steps performed to discover and visualize S11 data. This visual representation included estimations of the steps where more time was spent on.

Finally, the answers to the questionnaire were analyzed qualitatively following a Thematic Content Analysis (TCA). This type of qualitative analysis focuses on finding common themes in the interviews undertaken [@anderson_thematic_2007]. The extraction of common patterns within the interviews was initially done using a large language model (i.e. Chat-GPT 3.5 [@openai_chatgpt_2023]) using the prompt presented on [@sec-gpt-prompt; @openai_chatgpt_2023]. Moreover, the themes identified were further refined based on the interviewer's interpretation.

## Data and service integration

To efficiently integrate tools for big geo-spatial data discovery and visualization, a series of steps had to be followed. Initially, the datasets were selected. Subsequently, the structure of the catalog was defined. Following this, a Git repository containing the code required to generate the catalog was created. Static JSON files were then utilized to construct a dynamic STAC API. Ultimately, this API was deployed alongside other services using a continuous integration (CI) and continuous deployment (CD) pipeline. A further explanation of each step is presented in the following subsections.

### Dataset selection

Due to the desire of the company to continue moving towards a cloud-based workflow. The datasets that were considered for the catalog, were composed of either COGs or Zarrs. Nevertheless, since some of the data in the company is stored as virtual rasters (VRTs), methods to also index this type of data formats in the STAC catalog were included. Specifically, S11's long term goal is to store in the catalog datasets that can be classified as follows:

-   Static raster data
    -   Forest baselines (Stored as COGs)
    -   Third-party data (Stored as VRTs, Tiffs, or other formats)
-   DPROF results
    -   Results of continuous deforestation monitoring (Stored as ZARRs)
    -   Other DPROF results
-   Supply chain data (Vector data)
-   Complaince data (Vector data)

Nevertheless, the scope of this internship was limited to raster datasets. Therefore, the creation of the catalog was done using a limited amount of raster layers and they were incorporated as a proof of concept of how the catalog could be created.

### Proposed Catalog structure

The structure of the STAC catalog proposed can be seen on @fig-stac-str. In it, a selection of datasets that should be referenced in the catalog is presented and a hierarchical structure composed of thematic collections is suggested. This structure was not followed in the creation of the proof-of-concept catalog, as the purpose of this catalog was only to demonstrate the process of creating it. The final version of the structure will be determined by the company.

![Proposed STAC structure](img/STAC_Satelligence_structure.png){#fig-stac-str width="90%"}

### S11-cats repository

The [s11-cats repository](https://gitlab.com/satelligence/s11-cats) created is composed of a module named `cats` which consists of five submodules described in @tbl-cats-modules. Moreover, an overview of the main workflow followed in the main function of s11-cats is presented on @fig-s11-cats.

| **Submodule**       | **Description**                                                                          |
|----------------------------------|--------------------------------------|
| *gcs_tools*         | Module with functions to interact with data stored at Google Cloud Storage               |
| *general_metadata*  | Module to extract general metadata for a STAC item.                                      |
| *get_spatial_info*  | Module to get all spatial information from assets.                                       |
| *get_temporal_info* | Module with functions to extract temporal metadata of a dataset.                         |
| *stac_tools*        | Module with the functions to initialize a STAC, add collections, items and assets to it. |

: Description of cats submodules {#tbl-cats-modules}

![S11- cats main function](img/s11-cats.png){#fig-s11-cats}

As observed, the code in the repository requires a dictionary containing collection titles, descriptions, and tags, along with a list of links for each item to be added to each collection. It then generates two JSON files: one storing the collections' information and the other storing the items' information. This decision to produce two JSON files was made to facilitate the transition from the static catalog that has been created to the dynamic catalog that is desired.

### eoAPI + other services {#sec-eoapi}

Once a static catalog has been created, the next step involves developing the dynamic catalog by leveraging [eoAPI](https://eoapi.dev/) [@sarago_developmentseedeoapi_2024]. [eoAPI](https://eoapi.dev/) is a robust tool designed for managing, discovering and visualizing Earth observation data. It integrates several services that include indexing of large STAC collections and items using a Postgres database (See [PgSTAC](https://github.com/stac-utils/pgstac)), creating a dynamic catalog that can query the Postgres database (See [STAC API](https://github.com/stac-utils/stac-fastapi)) and two additional services for visualizing raster (See [Titiler-PgSTAC](https://github.com/stac-utils/titiler-pgstac)) and vector data (See [TiPg](https://github.com/developmentseed/tipg)).

[eoAPI](https://eoapi.dev/) integrates all of these services by using containerized versions that are able to communicate seamlessly with each other. A container is a lightweight, standalone, and executable package of software that includes everything needed to run an application. Containerizing the services facilitates deployment to the cloud using Google Kubernetes Engine (GKE). Kubernetes is an open-source platform designed for automating the deployment, scaling, and management of containerized applications [@poulton_kubernetes_2023]. It offers various advantages, such as scalability, efficient resource utilization, and simplified maintenance, making it an ideal solution for managing the dynamic catalog and the integrated services in a cloud environment.

Since the current version of [eoAPI](https://eoapi.dev/) does not include some extra services that were necessary to deploy, a separate containerized version of these services was deployed in the same K8 cluster. Notably, a version of [STAC Browser](https://github.com/radiantearth/stac-browser) and [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) to browse the catalog created and visualize Zarr datasets respectively.

### CI/CD pipeline

Finally, a gitlab CI/CD pipeline was created to automate the creation of the catalog using the [s11-cats repository](https://gitlab.com/satelligence/s11-cats), the deployment of eoAPI and extra services and the ingestion of the catalog into the deployed version of the dynamic catalog.

### Comparison with baseline scenario

Once a version of all of the services integrated was deployed online, the ease of discovery and visualization was again qualitatively analyzed by evaluating the steps processed for both finding and visualizing S11 data. These steps were then represented in a flowchart that could be compared to the one created on @sec-baseline.

## Multi-format data visualization

To assess the performance of dynamic tiling services for visualizing Cloud Optimized GeoTIFFs (COGs) and Zarr data formats, the following approach was undertaken. Firstly, a COG containing forest baseline information for the Riau region of Indonesia was used to create a series of Zarr files, each representing different overviews corresponding to various zoom levels. This pre-processing step, completed by the company prior to the study, ensured that the same data was used across both data formats, allowing for direct comparison. Then, the [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) service was then customized to work with the specific folder structure of the ZARR overviews previously created. Moreover, containerized versions of both [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) (for Zarr files) and [TiTiler-PgSTAC](https://github.com/stac-utils/titiler-pgstac) (for COG files) were locally deployed. The performance was measured by recording the response times for random tile requests at zoom levels ranging from 9 to 18. Finally, to mitigate the influence of cached data on response times, each iteration used a different colormap, with a total of twelve colormaps employed. This methodology enabled a systematic evaluation of the performance differences between the two data formats in a geo-spatial data visualization context.

### Speed up

The performance of both TiTiler services to dynamically create tiles for the different data formats was evaluated using the Speed Up metric proposed in @durbha_advances_2023 (@eq-speed-up). In this case, the Speed Up explains how much did the process of requesting tiles sped up by using a data format A compared to using a data format B.

$$ SpeedUp = \frac{t_{format A}}{t_{format B}} $$ {#eq-speed-up}

### Zoom level influence

Finally, the effect of the level of zoom in a web map visualization on the response times of requesting tiles from the different tiling services was evaluated by fitting an Ordinary Least Squares (OLS) univariate linear regression that followed @eq-lin-reg.

$$ ResponseTime = \beta_1 \cdot ZoomLevel + \beta_0 + \epsilon $$ {#eq-lin-reg}