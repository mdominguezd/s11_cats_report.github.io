

## Baseline scenario

The baseline scenario was defined as the set of methods currently being used by members of different teams at Satelligence to find, retrieve and visualize spatial data. This baseline scenario was evaluated qualitatively by interviewing four members of two different teams in the company (i.e. The data and the operations team). To keep a balance regarding experience of the study subjects, both the newest member of each team and a member with at least three years in the company were interviewed.

The questions asked during the interviews were oriented towards two main topics that were covered during this internship: Spatial data discovery and spatial data visualization. For both topics, the questions were divided into questions related to raster and vector datasets. The questions included in the interview can be found in @sec-baseline-q and were meant to be open questions with multiple possible answers that could elucidate qualitatively the current struggles faced by the members of different teams in Satelligence.

## Data and service integration

Briefly describe what will be included in the section: Explanation of the datasets included in the catalog, the building of the catalog (s11-cats)

### Datasets included

### S11-cats repository

Description of modules

#### Proposed Catalog structure

The structure of the STAC catalog proposed can be seen on @fig-stac-str. 

In it a selection of datasets that will be referenced in the catalog is presented and a hierachical structure composed of thematic collections is suggested. Moreover, the selection of the [STAC extensions](https://stac-extensions.github.io/)[^1] used for each dataset will be defined in this step. 

![Initial proposed STAC structure](img/STAC_Satelligence_structure.png){#fig-stac-str width='90%'}

[^1]: STAC extensions are additional metadata properties that can be added to a dataset. (e.g. Classes, bands, sensor-type, etc.)

### PgSTAC

### eoAPI 

-   Use of docker containers to run individual applications that can connect to each other.

### CI pipeline


```{=html}
<!-- ### Local STAC creation and browsing

This step will mainly be focused on the set up of the developing environment to both create a local STAC catalog and browse through it. This will include:

1. Creation of a virtual environment or Docker container with all the required packages to create a STAC catalog.
2. Creation of Local STAC using sample data from the company.
3. Browse through the STAC catalog using tools like STAC browser.

The **deliverable** of this step will be a GitLab repository with code to create a catalog, add assets from a local directory and browse through them locally using STAC browser.

### Organization of main STAC structure & extensions per asset {#sec-structure}

In this step the structure of the STAC catalog will be defined. This will involve the selection of datasets that will be referenced on the catalog, the definition of subcatalogs and/or collections to group items with similar metadata. An initial idea of the structure of the main STAC catalog can be seen on @fig-stac-str. Initially, the creation of two different subcatalogs is proposed to keep the static and dynamic dataset separated. Moreover, the selection of the [STAC extensions](https://stac-extensions.github.io/)[^1] used for each dataset will be defined in this step. 

![Initial proposed STAC structure](img/STAC_Satelligence_structure.png){#fig-stac-str width='90%'}

[^1]: STAC extensions are additional metadata properties that can be added to a dataset. (e.g. Classes, bands, sensor-type, etc.)

### Build main STAC v.0.1

This step will focus on the building of the initial version of the main STAC catalog, once the datasets and the overall structure has been defined. It will involve the population of the catalog with STAC components following the defined structure on @sec-structure. Furthermote, on this step a series of validation tools will be used to check that the STAC catalog created is followins the STAC spcification. These tools are part of the python package [stac-tools](https://github.com/stac-utils/stactools). 

The **deliverable** of this step will be a GitLab repository with code to create a catalog, create collections, add assets from a directory on the cloud and update them.

### Set up APIs on GCP

In this step a version of the STAC Browser application will be deployed using the tools from Google Cloud Platform (GCP). This application will allow users to browse and interact with the STAC catalog through a user-friendly interface. Additionally, this step will involve the definition of resources and tools from GCP that will be employed to deploy the application. For instance, the decision of doing it through a virtual machine or on a containerized way will be made.

The **deliverable** of this step will be a the STAC browser application running on GCP.

### Automate processes via CI pipeline

Finally, the code to create, modify and/or deploy the STAC catalog will be merged into a continuous integration pipeline that will allow the integration of this catalog with other tools from the company. For instance, the Distributed Processing Framework (DPROF), which is satelligence's Satellite Data Processing engine.  -->
```
```{=html}
<!-- ### Visualization tool development (Optional)

Finally, if time allows, an internal application will be developed to access and visualize the data from the STAC catalog created.

Required libraries:

-   [Streamlit](https://docs.streamlit.io/) for user interface

-   [Leafmap](https://leafmap.org/) for spatial data visualization

**Deliverable:** Containerized application to visualize the data  -->
```
## Multi-format data visualization

To assess the performance of dynamic tiling services for visualizing Cloud Optimized GeoTIFFs (COGs) and Zarr data formats, the following approach was undertaken. Firstly, a COG containing forest baseline information for the Riau region of Indonesia was used to create a series of Zarr files, each representing different overviews corresponding to various zoom levels. This preprocessing step, completed by the company prior to the study, ensured that the same data was used across both data formats, allowing for direct comparison. Then, the [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) service was then customized to work with the specific folder structure of the ZARR overviews previously created. Moreover, containerized versions of both [TiTiler-Xarray](https://github.com/developmentseed/titiler-xarray) (for Zarr files) and [TiTiler-PgSTAC](https://github.com/stac-utils/titiler-pgstac) (for COG files) were deployed locally. The performance was measured by recording the response times for random tile requests at zoom levels ranging from 9 to 18. Finally, to mitigate the influence of cached data on response times, each iteration used a different colormap, with a total of six colormaps employed. This methodology enabled a systematic evaluation of the performance differences between the two data formats in a geospatial data visualization context.

### Speed up

The performance of both TiTiler services to dynamically create tiles for the different data formats was evaluated using the Speed Up metric proposed in @durbha_advances_2023 (@eq-speed-up). In this case, the Speed Up explains how much did the process of requesting tiles sped up by using a data format A compared to using a data format B.

$$ SpeedUp = \frac{t_{format A}}{t_{format B}} $$ {#eq-speed-up}

### Zoom level influence

Finally, the effect of the level of zoom in a web map visualization on the response times of requesting tiles from the different tiling services was evaluated by fitting an Ordinary Least Squares (OLS) univariate linear regression that followed @eq-lin-reg. 

$$ ResponseTime = \beta_1 \cdot ZoomLevel + \beta_0 + \epsilon $$ {#eq-lin-reg}