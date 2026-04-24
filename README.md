# aiops_mlops_project

# About
This MLOps Project is about setting up a weather prediction system. It includes three pipelines: a feature pipeline, a training pipeline, and an inference pipeline. The feature pipeline updates weather data with data from the last 64 days (maximum the Open Meteo API returns with the free prediction API) and concatenates this data to create a dataset with all the available data since the pipeline was initially run for the first time.

# Features and Labels

### Features
- **Derived Features (from Open Meteo API):**
  - Humidity average over the last 24 hours
  - Change of barometric pressure in the last three hours

- **Realtime Features (calculated at pipeline runtime):**
  - Hour of the day
  - Month of the year

- **Randomly Generated Features (for inference):**
  - temperature_2m
  - relative_humidity_2m
  - precipitation
  - wind_speed_10m
  - cloud_cover
  - surface_pressure
  - dew_point_2m
  - pressure_msl

### Labels
- The target variable for prediction is the weather condition (e.g., sunny, rainy, cloudy).

# Pipelines

### Feature Pipeline
- **Purpose:** Updates weather data with the latest information from the Open Meteo API.
- **Features:** Derives humidity average and barometric pressure change.
- **Storage:** Uses a featurestore to store the processed features.

### Training Pipeline
- **Purpose:** Trains a machine learning model using the features from the featurestore.
- **Model:** Uses a Random Forest Classifier for weather prediction.
- **Input:** Features from the featurestore.
- **Output:** Trained model saved for inference.

### Inference Pipeline
- **Purpose:** Uses the trained model to predict weather conditions.
- **Input:** Realtime features and randomly generated features.
- **Output:** Predicted weather condition.

# FTI Architecture
- **Featurestore:** The feature pipeline stores processed features in a featurestore, ensuring consistency and reusability across pipelines.
- **Training:** The training pipeline retrieves features from the featurestore and trains the model.
- **Inference:** The inference pipeline uses the trained model and realtime features for prediction.

# Challenges and Limitations
- **Data Limitations:** The Open Meteo API provides limited historical data (64 days), which may affect model accuracy.
- **Model Precision:** Due to the limited data, the model may not achieve high precision.
- **Random Features:** Randomly generated features for inference may not fully represent real-world conditions.

# Setup and Execution

### Prerequisites
- Python 3.13
- Dependencies listed in `pyproject.toml`

### Installation
```bash
uv venv # To initiate a python 3.13 Version for the venv
source .venv/bin/activate.fish (or other activation scripts depending on OS and shell)
python -m ensurepip
pip install -r requirements.txt
```

### Running the Pipelines
For the pipelines to run successfully they have to be run consecutetively. 
Each run of the feature pipeline that creates new features will increment the common version with a new feature view. 
Existing versions of objects are not overwritten or deleted. 
1. **Feature Pipeline:**
   ```bash
   jupyter notebook F_Open_Meteo.ipynb
   ```

2. **Training Pipeline:**
   ```bash
   jupyter notebook T_Training_Pipeline.ipynb
   ```

3. **Inference Pipeline:**
   ```bash
   jupyter notebook I_Inference_Pipeline.ipynb
   ```

# Documentation and Reproducibility
- **Dependencies:** Defined in `pyproject.toml`.
- **Execution Steps:** Clearly outlined in the README.
- **Code Structure:** Organized into separate files for each pipeline.

# Reflexion and Limitation
- **Data Quality:** The model's performance is limited by the quality and quantity of data from the Open Meteo API.
- **Feature Engineering:** Randomly generated features may not capture real-world variability accurately.
- **Model Choice:** A Random Forest Classifier was chosen for its simplicity and interpretability, but other models may perform better with more data.
- **Versioning:** Feature Views, Feature Groups, Training Datasets, Models use the same version. The initial Idea was that it can be easily tracked which step has failed and that especially inferencing always uses a valid dataset. However there is no implemenation yet to hold back a new version and there is no quality gate. The extended use of labels could be a basic improvement.

# Expansion stages
- Hopsworks is used as a feature store. 
- No persistent local data is required except for the credentials. A lockenv could be used.
- Test and Train split is performed in hopsworks and locally for reference
- Trained models are deployed to hopsworks and pulled for inference


# Future Improvements
- **Data Collection:** Incorporate additional data sources to improve model accuracy.
- **Feature Engineering:** Use more sophisticated methods to derive features from raw data.
- **Model Tuning:** Experiment with different models and hyperparameters to improve performance.