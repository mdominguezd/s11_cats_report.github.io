## Baseline scenario

### Main findings

The main finding of the interviews were the steps followed currently to discover, retrieve and visualize data. These steps are summarized on @fig-baseline and show how complex and time consuming the process of discovering and visualizing spatial data can be for a Satelligence employee nowadays. Moreover, the steps followed were categorized in four classes depending on how much time is generally spent carrying out.

![Baseline workflow](img/Baseline_data_discovery_workflow.png){#fig-baseline width="100%"}

The major pitfalls found on the process of data discovery in the company could be summarized in ....

## Service integration

*Explain here how eoAPI uses multiple services, how each of them helps S11 in their data discovery and vizz tasks, and how did I manage to deploy it*

Kubernetes

STAC-API, pgSTAC, TiTiler

## Multi-format data visualization

TiTiler-PgSTAC & TiTiler-xarray

## Performance assessment

### Data discovery

### Data visualization
IDEA: GET requests and time them

```{python}
#| echo: false
#| fig-cap: "Request times depending on zoom level"

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import requests
import random

tiles = ["9/399/254", "10/800/505", "11/1603/1012",  "12/3209/2042", "13/6407/4075", "14/12817/8159", "15/25678/16271", "16/51268/32552", "17/102503/65134", "18/205062/130211"]

def modify_tile(tile):
    parts = tile.split('/')
    z = int(parts[0])
    x = int(parts[1])
    y = int(parts[2])

    # Determine the range of change based on the value of z
    if z <= 9:
        change_range = 2
    elif z <= 12:
        change_range = 5
    elif z <= 15:
        change_range = 10
    elif z <= 18:
        change_range = 50

    # Apply the change to x and y
    x_change = random.randint(-change_range, change_range)
    y_change = random.randint(-change_range, change_range)

    new_x = x + x_change
    new_y = y + y_change

    # Return the modified tile as a string
    return f"{z}/{new_x}/{new_y}"

times_zarr = []
times_cog = []

z_level = []

cmap = ["=[[[1.0,1.1],[0,47,0,255]],[[2.0,2.1],[55,76,33,255]],[[3.0,3.1],[105,140,60,255]],[[4.0,4.1],[178,199,140,255]],[[5.0,5.1],[164,198,121,255]],[[6.0,6.1],[198,112,85,255]],[[7.0,7.1],[170,219,167,255]],[[8.0,8.1],[87,162,164,255]],[[50.0,50.1],[255,183,1,255]],[[52.0,52.1],[238,223,201,255]],[[53.0,53.1],[185,120,119,255]],[[54.0,54.1],[218,170,241,255]],[[55.0,55.1],[40,205,167,255]],[[60.0,60.1],[208,227,243,255]],[[66.0,66.1],[166,219,204,255]],[[70.0,70.1],[255,255,255,255]],[[71.0,71.1],[185,136,94,255]],[[72.0,72.1],[125,165,142,255]],[[74.0,74.1],[188,85,123,255]],[[90.0,90.1],[241,195,132,255]]]","_name=greens_r&rescale=0,70"]

for i in range(1):

    mod_tiles = [modify_tile(tile) for tile in tiles]
    k = int(i/2-i//2 + 0.5)

    for tile in mod_tiles:

        url_zarr = f"http://localhost:8084/tiles/WebMercatorQuad/{tile}%401x?url=gs://s11-tiles/zarr/separate&variable=band_data&reference=false&decode_times=true&consolidated=true&colormap{cmap[k]}&return_mask=true"

        url_cog = f"http://localhost:8082/collections/Example%20FBL%20Riau/items/FBL_V5_2021_Riau_cog/tiles/WebMercatorQuad/{tile}%401x?bidx=1&assets=data&unscale=false&resampling=nearest&reproject=nearest&colormap{cmap[k]}&return_mask=true"

        x = requests.get(url_zarr)
        times_zarr.append(x.elapsed.total_seconds())

        x = requests.get(url_cog)
        times_cog.append(x.elapsed.total_seconds())

        z_level.append(int(tile.split('/')[0]))

sns.set_style('ticks')

fig = plt.figure(figsize = (10,7))

sns.regplot(x = z_level, y=times_cog)
sns.regplot(x = z_level, y=times_zarr)

Z_level = [12, 18, 14]

COG_t = times_cog #[2.97, 3.93, 3.45]
ZARR_t = times_zarr #[5.90, 7.74, 6.43]

data = pd.DataFrame([COG_t, ZARR_t]).T
data.columns = ['COG', 'ZARR']

fig = plt.figure(figsize= (10,5))

ax = sns.boxplot(data, palette = 'deep')
sns.despine(trim = True, offset = -10)

a = ax.set_ylabel('Render time [s]')

print("Speed up (COG)", data['ZARR'].mean()/data['COG'].mean())
```



