---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

(chapter02)=

# Creating Interactive Maps

```{contents}
:local:
:depth: 2
```

## Introduction

There are numerous Python packages available for interactive mapping and geospatial analysis {cite}`Wu2021-iv`. However, each package has its own unique API for creating maps and visualizing data, which can be difficult for beginners to navigate. Geemap simplifies the process by providing a unified API interface for creating interactive maps and visualizing data, allowing users to switch between plotting backends with only one line of code.

Throughout this chapter, we will explore how to create interactive maps using one of five different plotting backends. We will also provide practical examples of how to add basemaps to an interactive map. With hundreds of basemaps available, they can be easily added to an interactive map using only a single line of code.

(chapter02:requirements)=

## Technical requirements

To follow along with this chapter, you will need to have geemap and several optional dependencies installed. If you have already followed {numref}`ch01:install` - _Installing geemap_, then you should already have a conda environment with all the necessary packages installed. Otherwise, you can create a new conda environment and install [pygis](https://pygis.gishub.org) with the following commands, which will automatically install geemap and all the required dependencies:

```bash
conda create -n gee python
conda activate gee
conda install -c conda-forge mamba
mamba install -c conda-forge pygis
```

Next, launch JupyterLab by typing the following commands in your terminal or Anaconda prompt:

```bash
jupyter lab
```

Alternatively, you can use geemap in a Google Colab cloud environment without installing anything on your local computer. Click [02_maps.ipynb](https://colab.research.google.com/github/giswqs/geebook/blob/master/chapters/02_maps.ipynb) to launch the notebook in Google Colab.

Once in Colab, you can uncomment the following line and run the cell to install pygis, which includes geemap and all the necessary dependencies:

```{code-cell} ipython3
# %pip install pygis
```

The installation process may take 2-3 minutes. Once pygis has been installed successfully, click the **RESTART RUNTIME** button that appears at the end of the installation log or go to the **Runtime** menu and select **Restart runtime**. After that, you can start coding.

To begin, import the necessary libraries that will be used in this chapter:

```{code-cell} ipython3
import ee
import geemap
```

Initialize the Earth Engine Python API:

```{code-cell} ipython3
geemap.ee_initialize()
```

If this is your first time running the code above, you will need to authenticate Earth Engine first. Follow the instructions in {numref}`ch01-ee-auth` - _Earth Engine authentication_ to authenticate Earth Engine.

## Plotting backends

**Geemap** has five plotting backends, including [ipyleaflet](https://github.com/jupyter-widgets/ipyleaflet), [folium](https://python-visualization.github.io/folium), [plotly](https://plotly.com), [pydeck](https://deckgl.readthedocs.io/en/latest), and [kepler.gl](https://docs.kepler.gl/docs/keplergl-jupyter). An interactive map created using one of the plotting backends can be displayed in a Jupyter environment, such as Google Colab, Jupyter Notebook, and JupyterLab. By default, `import geemap` will use the ipyleaflet plotting backend.

The five plotting backends supported by geemap do not offer equal functionality. The ipyleaflet plotting backend provides the richest interactive functionality, including the interactive Graphical User Interface (GUI) for loading, analyzing, and visualizing geospatial data interactively without coding. For example, users can add vector data (e.g., GeoJSON, Shapefile, KML, GeoDataFrame) and raster data (e.g., GeoTIFF, Cloud Optimized GeoTIFF [COG]) to the map with a few clicks. Users can also perform geospatial analysis using the WhiteboxTools GUI with 500+ geoprocessing tools directly within the map interface. Other interactive functionality (e.g., split-panel map, linked map, time slider, timeseries inspector) can also be useful for visualizing geospatial data. The ipyleaflet package is built upon ipywidgets and allows bidirectional communication between the frontend and the backend, enabling the use of the map to capture user input {cite}`QuantStack2019-bd`.

In contrast, folium has relatively limited interactive functionality. It is meant for displaying static data only. It can be useful for developing interactive web apps when ipyleaflet is not supported. Note that the aforementioned interactive GUI is not available for other plotting backends. Geemap provides a unified API that makes it very easy to switch from one plotting backend to another. To choose a specific plotting backend, use one of the following:

- `import geemap.geemap as geemap`
- `import geemap.foliumap as geemap`
- `import geemap.deck as geemap`
- `import geemap.kepler as geemap`
- `import geemap.plotlymap as geemap`

### Ipyleaflet

You can simply use `geemap.Map()` to create an interactive map with the default settings. First, let's import the `geemap` package:

```{code-cell} ipython3
import geemap
```

Next, create an interactive map using the ipyleaflet plotting backend. The [geemap.Map](https://geemap.org/geemap/#geemap.geemap.Map) class inherits the [ipyleaflet.Map](https://ipyleaflet.readthedocs.io/en/latest/map_and_basemaps/map.html) class. Therefore, you can use the same syntax to create an interactive map as you would with `ipyleaflet.Map`.

```{code-cell} ipython3
Map = geemap.Map()
```

This code creates a new map and assigns it to a new variable named `Map`. To display the map in a Jupyter notebook, simply type the variable name:

```{code-cell} ipython3
Map
```

Keep in mind that throughout this book, `Map` is commonly used to refer to the interactive map. It is just a variable name. You can use whatever name you want (e.g., `m`) as long as it complies with the following Python variable names rules:

- A variable name must start with a letter or the underscore character
- A variable name cannot start with a number
- A variable name can only contain alphanumeric characters and underscores (A-z, 0-9, and \_ )

In general, a Python variable name should be lowercase. The reason we use `Map` rather than `m` is because in the Earth Engine JavaScript API, `Map` is a reserved keyword referring to the interactive map. We want to be consistent with the Earth Engine JavaScript API so that users can have an easier transition to geemap. Users are by no means required to use `Map` as the variable name for the interactive map.

To customize the map, you can specify various keyword arguments, such as `center` ([lat, lon]), `zoom`, `width`, and `height`. The default `width` is `100%`, which takes up the entire cell width of the Jupyter notebook. The `height` argument accepts a number or a string. If a number is provided, it represents the height of the map in pixels. If a string is provided, the string must be in the format of a number followed by `px`, e.g., `600px`.

```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4, height=600)
Map
```

The default map comes with all the following controls (see {numref}`ch02_ipyleaflet`):

- `attribution_ctrl`
- `data_ctrl`
- `draw_ctrl`
- `fullscreen_ctrl`
- `layer_ctrl`
- `measure_ctrl`
- `scale_ctrl`
- `toolbar_ctrl`
- `zoom_ctrl`

```{figure} images/ch02_ipyleaflet.jpg
---
name: ch02_ipyleaflet
width: 100%
---
A map created using the ipyleaflet plotting backend.
```

To hide a control, set `control_name` to `False`, e.g., `draw_ctrl=False`.

```{code-cell} ipython3
Map = geemap.Map(data_ctrl=False, toolbar_ctrl=False, draw_ctrl=False)
Map
```

You can also set `lite_mode=True` to show only the Zoom Control.

```{code-cell} ipython3
Map = geemap.Map(lite_mode=True)
Map
```

To save the map as an HTML file you can call the `save` method:

```{code-cell} ipython3
Map.save('ipyleaflet.html')
```

### Folium

To create an interactive map using the folium plotting backend, simply import the library as follows:

```{code-cell} ipython3
import geemap.foliumap as geemap
```

Then, you can use the same line of code to create and display an interactive map ({numref}`ch02_folium`) as you would with the ipyleaflet plotting backend introduced above.

```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4, height=600)
Map
```

```{figure} images/ch02_folium.jpg
---
name: ch02_folium
width: 100%
---
A map created using the folium plotting backend.
```

Folium does not support bidirectional communication {cite}`QuantStack2019-bd`. Once the map is created and displayed in a notebook cell, you can't modify the map's properties, e.g., add/remove layers, add controls, change width/height. Also, the data and toolbar controls available in ipyleaflet (see {numref}`ch02_ipyleaflet`) are not supported in folium as folium does not support ipywidgets.

To save the map as an HTML file we can call the `save` method:

```{code-cell} ipython3
Map.save('folium.html')
```

### Plotly

To create an interactive map using the plotly plotting backend, simply import the library as follows:

```{code-cell} ipython3
import geemap.plotlymap as geemap
```

Then, create and display an interactive map ({numref}`ch02_plotly`).

```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_plotly.jpg
---
name: ch02_plotly
width: 100%
---
A map created using the plotly plotting backend.
```

Note that you might not see the map displayed in Colab as it does not yet support plotly FigureWidget. If you run into an error saying `FigureWidget - 'mapbox._derived' Value Error` (see [plotly issue 2570](https://github.com/plotly/plotly.py/issues/2570#issuecomment-738735816)), uncomment the following line and run it.

```{code-cell} ipython3
# geemap.fix_widget_error()
```

### Pydeck

To create an interactive map using the pydeck plotting backend, simply import the library as follows:

```{code-cell} ipython3
import geemap.deck as geemap
```

Then, create and display an interactive map ({numref}`ch02_pydeck`).

```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_pydeck.jpg
---
name: ch02_pydeck
width: 100%
---
A map created using the pydeck plotting backend.
```

### KeplerGL

To create an interactive map using the [kepler.gl](https://docs.kepler.gl/docs/keplergl-jupyter) plotting backend, simply import the library as follows:

```{code-cell} ipython3
import geemap.kepler as geemap
```

Then, create and display an interactive map ({numref}`ch02_kepler`).

```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_kepler.jpg
---
name: ch02_kepler
width: 100%
---
A map created using the KeplerGL plotting backend.
```

## Adding basemaps

There are several ways to add basemaps to a map. You can specify the basemap to use in the `basemap` keyword argument when creating the map with the `geemap.Map()` method. Alternatively, you can add basemap layers to the map using the `Map.add_basemap()` method. Geemap has hundreds of built-in basemaps available through [xyzservices](https://github.com/geopandas/xyzservices) that can be easily added to the map with only one line of code.

### Built-in basemaps

Let's try out some of the built-in basemaps. First, import the geemap library as follows:

```{code-cell} ipython3
import geemap
```

Next, create a map by specifying the basemap to use as follows. For example, the `HYBRID` basemap represents the Google Satellite Hybrid basemap ({numref}`ch02_basemap_hybrid`).

```{code-cell} ipython3
Map = geemap.Map(basemap='HYBRID')
Map
```

```{figure} images/ch02_basemap_hybrid.jpg
---
name: ch02_basemap_hybrid
width: 100%
---
The Google Satellite Hybrid basemap.
```

You can add as many basemaps as you like to the map. For example, the following code adds the `OpenTopoMap` basemap to the map above ({numref}`ch02_basemap_opentopomap`):

```{code-cell} ipython3
Map.add_basemap('OpenTopoMap')
```

```{figure} images/ch02_basemap_opentopomap.jpg
---
name: ch02_basemap_opentopomap
width: 100%
---
The OpenTopoMap basemap.

```

To find out the list of available basemaps:

```{code-cell} ipython3
for basemap in geemap.basemaps.keys():
    print(basemap)
```

In total, there are 100+ basemaps available.

```{code-cell} ipython3
len(geemap.basemaps)
```

### XYZ tiles

You can also add XYZ tile layers to the map using the `Map.add_tile_layer()` method. For example, the following code creates an interactive map and adds the Google Terrain basemap to it:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_tile_layer(
    url="https://mt1.google.com/vt/lyrs=p&x={x}&y={y}&z={z}",
    name="Google Terrain",
    attribution="Google",
)
Map
```

### WMS tiles

Similarly, you can add WMS tile layers to the map using the `Map.add_wms_layer()` method. For example, the following code creates an interactive map and adds the National Land Cover Database (NLCD) 2019 basemap to it ({numref}`ch02_basemap_nlcd`):

```{code-cell} ipython3
Map = geemap.Map(center=[40, -100], zoom=4)
url = 'https://www.mrlc.gov/geoserver/mrlc_display/NLCD_2019_Land_Cover_L48/wms?'
Map.add_wms_layer(
    url=url,
    layers='NLCD_2019_Land_Cover_L48',
    name='NLCD 2019',
    format='image/png',
    attribution='MRLC',
    transparent=True,
)
Map
```

```{figure} images/ch02_basemap_nlcd.jpg
---
name: ch02_basemap_nlcd
width: 100%
---
The National Land Cover Database (NLCD) 2019 basemap.
```

Want to find out more freely available WMS basemaps? Check out the [USGS National Map Services](https://apps.nationalmap.gov/services). Once you get a WMS URL, you can follow the example above to add it to the map.

### Planet basemaps

[Planet Labs](https://www.planet.com) provides high-resolution global satellite imagery with a high temporal frequency. The monthly and quarterly global basemaps can be streamed via the [XYZ Basemap Tile Service](https://developers.planet.com/docs/basemaps/tile-services/xyz) for use in web mapping applications or for visualization purposes. A valid [Planet account](https://developers.planet.com/docs/apis/data/#how-to-become-a-planet-developer) is required to access the Basemap Tile Service. Once you sign up for a Planet account, you can get your API key by navigating to the [Account Settings page](https://www.planet.com/account/#/user-settings) as shown in ({numref}`ch02_planet_api_key`).

```{figure} images/ch02_planet_api_key.jpg
---
name: ch02_planet_api_key
width: 100%
---
Planet account profile page.
```

Replace `YOUR_API_KEY` below with the API key copied from your Account Settings page shown above. You can create an environment variable called `PLANET_API_KEY` and set it to the API key so that you can access the basemap tiles without having to specify the API key every time.

```{code-cell} ipython3
import os

os.environ["PLANET_API_KEY"] = "YOUR_API_KEY"
```

First, let's look at the list of available quarterly basemaps. Planet has quarterly basemaps going back to the first quarter of 2016. The names of the quarterly basemaps are: `Planet_2016q1`, `Planet_2016q2`, ..., `Planet_2022q1`, and so on.

```{code-cell} ipython3
quarterly_tiles = geemap.planet_quarterly_tiles()
for tile in quarterly_tiles:
    print(tile)
```

Next, let's look at the list of available monthly basemaps. Planet has monthly basemaps going back to January 2016. The names of the monthly basemaps are: `Planet_2016_01`, `Planet_2016_02`, ..., `Planet_2022_04`, and so on.

```{code-cell} ipython3
monthly_tiles = geemap.planet_monthly_tiles()
for tile in monthly_tiles:
    print(tile)
```

To add a monthly basemap to the map, use the `Map.add_planet_by_month()` method as follows:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_planet_by_month(year=2020, month=8)
Map
```

To add a quarterly basemap to the map, use the `Map.add_planet_by_quarter()` method as follows:

```{code-cell} ipython3
Map = geemap.Map()
Map.add_planet_by_quarter(year=2019, quarter=2)
Map
```

The map should look like {numref}`ch02_basemap_planet`.

```{figure} images/ch02_basemap_planet.jpg
---
name: ch02_basemap_planet
width: 100%
---
The Planet quarterly basemap for second quarter of 2019.

```

### Basemap GUI

Geemap makes it easy for users to add basemaps to their maps without having to write any code. By clicking the "map" icon on the toolbar, the basemap GUI is activated ({numref}`ch02_basemap_gui`). Simply click the dropdown menu to select the basemap that you want to add. Additionally, if you have created an environment variable called `PLANET_API_KEY` and set it to your Planet API key, the Planet basemaps will also be available to select from the dropdown menu.

To set the Planet API key, use the following code:

```{code-cell} ipython3
import os

os.environ["PLANET_API_KEY"] = "YOUR_API_KEY"
```

Then the Planet basemaps will be available via the basemap dropdown menu.

```{code-cell} ipython3
Map = geemap.Map()
Map
```

```{figure} images/ch02_basemap_gui.jpg
---
name: ch02_basemap_gui
width: 100%

---
The basemap GUI for changing basemaps interactively without coding.
```

## Summary

In this chapter, we began by introducing the fundamentals of Jupyter notebooks. We then provided hands-on examples of using the five geemap plotting backends to create interactive maps. Additionally, we explored how to incorporate various basemap layers, such as built-in basemaps, XYZ tiles, WMS tiles, and Planet basemaps, into our maps.

By now, you should be proficient in using Jupyter notebook to generate interactive maps and switch between various basemaps seamlessly. That concludes this chapter. In the next chapter, we will delve into Google Earth Engine data.
