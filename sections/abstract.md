# Executive summary {-}

This internship report examines the challenges and opportunities in data discovery and visualization for Satelligence, a company specializing in the use of earth observation data for detecting environmental risks and facilitate the migration towards sustainable supply chains. With the rapid growth of geospatial data, efficient access and visualization tools are crucial. In this report, the current methods employed by the company for data discovery and visualization were analysed qualitatively, the integration of cloud-optimized data formats, cloud services, and STAC specifications to improve these processes were explored and the performance of dynamic tiling services in visualizing various cloud-optimized data formats was assessed. The results of the qualitative analysis showed that Satelligence's current methods are inefficient, limit accessibility and underscore the need of a user-friendly data discovery and visualization service. Moreover, the report shows tha the integration of technologies like: transitioning to cloud-optimized data formats like Cloud-Optimized Geotiffs (COGs) and Zarrs, using a dynamic STAC API standard for data organization, employing dynamic tiling libraries for optimized web visualization and deploying services using Google Kubernetes Engine was able to enhance the process of discovering and visualizing data within the company. Finally, the report suggests that dynamic tiling services perform better with COG tiles, which are 2.53 times faster to request than Zarr tiles, which aligns with COGs' optimization for spatial data visualization. Nevertheless, new improvements and developments in the community could enhance Zarr performance in the future.


:::{.content-visible when-format="pdf"}
\vspace*{\fill} 

An interactive version of this report can be found in [https://mdominguezd.github.io/s11_cats_report.github.io/](https://mdominguezd.github.io/s11_cats_report.github.io/).

:::

\newpage

# List of abreviations {-}

| **Abreviations** | **Description**                                                                                          |
|----------------------------------------|--------------------------------|
 EUDR | European Union Deforestation Regulation |
 STAC | Spatio-Temporal Asset Catalog |
 COG  | Cloud-Optimized GeoTiff |  
 OGC  | Open Geospatial Consortium |
 SDI  | Spatial Data Infrastructure |
 S11  | Satelligence |
 K8s  | Kubernetes |
 DPROF| Distributed Processing Framework |
 JSON | JavaScript Object Notation |
 API  | Application Programming Interfaces |
 HTTP | HyperText Transfer Protocol |
 SQL  | Standard Query Language |
 FBL  | Forest Baseline |
 DEM  | Digital Elevation Map |
 TCA  | Thematic Content analysis |
 VRT  | Vitual Raster |
: Abbreviation list {.striped .hover}
