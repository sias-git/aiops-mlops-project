# aiops_mlops_project

# About
This MLOps Project is about setting up a weather prediction.
It has a Feature Pipeline to update the weather data with data from the last 64 days (maximum the Open Meteo API returns with the free prediction API) and concatenates this data to create a dataset with all the available data since the pipeline was initally run for the first time.
There will be multiple features derived:
- hour of the day
- month of the year
- humidity average
- change of barometric pressure in the last three hours
During prediction the following features are viewed as Realtime features:
- Cloud Coverage percentage
- Hour of the day
- Month of the year

Due to the limited data available the model will probably have a not so great precision.


# Initialization
```
uv venv # To initiate a python 3.13 Version for the venv
source .venv/bin/activate.fish
python -m ensurepip
python install -r requirements.txt
```

