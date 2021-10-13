# Model Insight Patterns - Regrid

This directory contains various Python patterns for model post-processing regridding whereby the geographical scale of model output is modified. These patterns are designed to improve modelers' ability to derive insights and identify impacts from their models' native outputs. 


# Requirements

Each Jupyter notebook in this directory requires some or all of the following Python libraries:
```
geopandas==0.9.0
matplotlib==3.4.2
netCDF4==1.5.7
numpy==1.20.3
rioxarray==0.7.1
xarray==0.18.2
```

# Data Files

The examples use three (3) data sources:
1. `GADM`
2. `LPJmL model sample output`
3. `Sea Surface Temperature Data`

## GADM

The Database for Global Administrative Areas ([GADM](https://gadm.org/data.html)) provides geometries that can be used to regrid model output to countries of interest.

The GADM administrative area zero shape file for countries of the world can be downloaded and unziped using the following shell commands:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/gadm_0.zip
unzip gadm_0.zip
rm *.zip
```

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

## Sea Surface Temperature Data

This NetCDF file contains global sea surface temperatures for 24 time points.

It can be downloaded from S3 with the following shell command:
```
wget https://jataware-world-modelers.s3.amazonaws.com/analytic-layers/sample_2.nc
```


# Examples

There are two (2) regrid examples:

1. `regrid-fixed-cell-size-interpolate`
2. `regrid-reference-cell-size-interpolate`


## regrid-fixed-cell-size-interpolate

A sample NetCDF file containing global sea surface temperatures for 24 time points is used. We calculate the scale in km of the dataset, and in our use case want to increase the scale to reduce the size of the dataset and match the scale of another hypothetical dataset. 


## regrid-reference-cell-size-interpolate

A sample NetCDF file, based on the LPJmL model's output, is clipped to the GADM geometries of countries of interest. The filtered model output is regridded to a higher scale resolution using interpolation and the total counts corrected for the change in scale.



