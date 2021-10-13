# Model Insight Patterns - Analytics

This directory contains various Python patterns for model post-processing analytics. These patterns are designed to improve modelers' ability to derive insights and identify impacts from their models' native outputs. 


# Requirements

Each Jupyter notebook in this directory requires some or all of the following Python libraries:
```
cdo==1.5.5
geopandas==0.9.0
matplotlib==3.4.2
netCDF4==1.5.7
numpy==1.20.3
rioxarray==0.7.1
xarray==0.18.2
```
You'll need to [install CDO](https://code.mpimet.mpg.de/projects/cdo/wiki#Installation-and-Supported-Platforms) (Climate Data Operators command line suite) before implementing the Python cdo library. Make sure to follow instructions for your specific OS.

# Data Files

The examples use five (5) data sources:
1. `GADM`
2. `Land-use NetCDF`
3. `GPW Population Data`
4. `MaxHop model sample output`
5. `LPJmL model sample output`

## GADM

The Database for Global Administrative Areas ([GADM](https://gadm.org/data.html)) provides geometries that can be used to regrid model output to countries of interest.

The GADM administrative area zero shape file for countries of the world can be downloaded and unziped using the following shell commands:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/gadm_0.zip
unzip gadm_0.zip
rm *.zip
```

## Land-use NETCDF

The USGS [MODIS Land Use](https://lpdaac.usgs.gov/products/mcd12q1v006/) data product provides high resolution (500m) land type and land use data.

The original `MODIS/Terra+Aqua Land Cover Type Yearly L3 Global 500 m SIN Grid` has been regridded using a `nearest` interpolation approach to 5 km resolution and clipped to Africa. The original data from MODIS can be [found here](https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/MCD12Q1.006_500m_aid0001.nc) at the 500 m resolution.

The land-use NetCDF file can be downloaded from S3 with the following shell command:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/land-use-5km.nc
```

## GPW Population Data

The SEDAC Gridded Popolation of the World([GPW](https://sedac.ciesin.columbia.edu/data/collection/gpw-v4)), v4 collection models the distribution of human population on a continuous raster surface that can be merged with model output. 

GPW download requires sign up at [GPW 4 access here](https://sedac.ciesin.columbia.edu/data/collection/gpw-v4).


## MaxHop model sample output

A sample GeoTIFF file based on the MaxHop model's output can be downloaded with the following shell command:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/locust_sample.tif
```

The model output geography is limited Ethiopia between roughly latitudes 3.5-14.9 and longitudes 33.0-48.0.


## LPJmL model sample output

A sample NetCDF file based on the LPJmL model's output can be downloaded with the following shell command:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/sample.nc
```
The example NetCDF has the following Dimensions:
```
time: 4 crop: 26 latitude: 280 longitude: 720
```
and Data variables: 
```
harvested_area, harvest, production, irrigation_water, sdate, hdate, NameCrop, weather_year
```

The time variables are dropped in the Python patterns notebooks due to issues when clipping the dataset with these variables present.


# Examples

There are four (4) analytics examples:

1. `land-use-analytics-locust-cropland`
2. `land-use-analytics`
3. `population-analytics-locusts`
4. `population-analytics`


## land-use-analytics-locust-cropland

This example starts with sample GeoTIFF output from the MaxHop model and the MODIS land use dataset. The GADM shape file is loaded, filtered for Ethiopia, and used to clip the MODIS and model output dataset to only Ethiopia. CDO is used to remap the MODIS data to the same grid resolution as the model output, using a nearest-neighbor method because the MODIS data is categorial. The regridded modis dataset is then merged with the model output for Ethiopia only. This allows us to plot locust extent by land cover type, for example crop lands. 

## land-use-analytics

A sample NetCDF file, based on the LPJmL model's output, and the MODIS land use dataset are loaded and clipped to the GADM geometries of countries of interest. The filtered MODIS dataset is regridded to match the filtered model output using a CDO nearest-neighbor method because the MODIS data is categorial, and this regridded MODIS dataset is merged with the filtered model output. This allows us to plot crop production by land use properties.



## population-analytics-locusts

This example starts with sample GeoTIFF output from the MaxHop model and the GPW popluation data. These datasets are filtered for Ethiopia using the GADM shape file. The filtered GPW dataset is regridded using CDO to match the filtered model output, and these are merged. Using the final merged dataset, we are able to plot population affected at an arbritary locust extent threshold.



## population-analytics

This example starts with a sample NetCDF file based on the LPJmL model's output and the GPW population data. These datasets are filtered for countries of interest using the GADM shape file, and the filtered GPW dataset is regridded to the filtered model output using CDO. After the regridded GPW data is merged with the filtered model output, we add a production-per-capita variable and plot this.