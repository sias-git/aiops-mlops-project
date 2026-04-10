# aiops_mlops_project

# About
This MLOps Project is about setting up a weather prediction.
It has a Feature Pipeline to update the weather data with data from the last 64 days (maximum the Open Meteo API returns with the free prediction API) and concatenates this data to create a dataset with all the available data since the pipeline was initally run for the first time.

# Features and Labels


# Pipelines

There will be multiple features derived from an api call to open meteo at pipeline runtime:
- humidity average over the last 24 hours
- change of barometric pressure in the last three hours

During prediction the following features are viewed as Realtime features as they are calculated at pipeline runtime:
- Hour of the day
- Month of the year

The following features will be randomly generated for inference for the scope of this project. Technically they could also be used from the opem meteo call. But this way we get some variation for repeated calls.
- temperature_2m 
- relative_humidity_2m
- precipitation
- wind_speed_10m
- cloud_cover
- surface_pressure
- dew_point_2m
- pressure_msl

Due to the limited data available the model will probably have a not so great precision.


# Initialization
```
uv venv # To initiate a python 3.13 Version for the venv
source .venv/bin/activate.fish
python -m ensurepip
python install -r requirements.txt
```