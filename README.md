# Predicting the Severity of Car Accidents in Zurich City

This repository contains the final project for the course **3,580: Workshop Fundamentals of Data Science** at the **University of St.Gallen (HSG)**.  
The project investigates whether the severity of road traffic accidents in the City of Zurich can be predicted using accident characteristics, temporal information, geospatial features, traffic volume, weather data, road infrastructure, and public transport proximity.

## Project Overview

The goal of this project is to build a binary classification model that predicts whether a recorded traffic accident resulted in:

- **Property damage only**
- **Personal injury**, including injuries and fatalities

The main use case behind the project is an emergency-response setting. For example, a future traffic camera system or autonomous vehicle could detect that an accident has occurred and estimate whether personal injuries are likely. A high-risk prediction could then support faster emergency-service notification.

The project uses road traffic accident data from the City of Zurich covering the period from **2012 to 2022**. The original accident dataset contains accident-level information such as accident type, involved parties, road type, accident location, and accident timing. We enrich this dataset with several external data sources to create a more informative feature set.

## Repository Structure

```text
.
├── data/                         # Reference for dataset download
├── notebook/                     # Main project notebook
├── presentation/                 # Final presentation as PDF
└── README.md                     # Repository overview
```

The repository is structured around the final notebook, the data folder, and the final project presentation. The notebook contains the full workflow from data preprocessing and feature engineering to exploratory data analysis, model training, and evaluation.

## Data Sources

The project combines several datasets related to accidents, weather, traffic, and urban infrastructure in Zurich.

| Data source | Main contribution |
|---|---|
| Road traffic accidents | Main dataset and target variable |
| Weather data | Hourly weather information from Zurich weather stations |
| Traffic volume data | Hourly motorized traffic counts from counting stations |
| Bicycle and pedestrian traffic counts | Counts from nearby measuring stations |
| Public transport stops | Distance to nearest bus, tram, train, or ferry stop |
| Traffic zones | Zone type, such as pedestrian zones, meeting zones, or speed-reduced areas |
| Road network and speed regimes | Nearest road segment, speed limit, and distance to intersections |
| Pedestrian and bicycle network | Distance to crossings and nearby bicycle infrastructure |
| Public holidays | Boolean indicator for federal, cantonal, or regional holidays in Zurich |

The final modelling dataset is created by linking the accident records to these additional datasets through time, location, and spatial proximity.

## Methodology

### 1. Data Preprocessing

The raw accident data did not contain a complete date variable. It only included the year, month, weekday, and hour of the accident. Since the observations were ordered chronologically, we reconstructed the exact accident date and created a full `DateTime` feature.

Coordinates were transformed into geospatial point objects. This made it possible to compute distances to infrastructure features such as public transport stops, traffic counting stations, pedestrian crossings, bicycle paths, and intersections.

### 2. Feature Engineering

A major part of the project consisted of extensive feature engineering. We added spatial, temporal, traffic-related, weather-related, and infrastructure-related variables to the main accident dataset.

Examples of engineered features include:

- Lagged traffic volume at the three nearest traffic counting stations
- Distance to the nearest traffic counting stations
- Hourly weather variables, including temperature, precipitation, wind, humidity, radiation, and air pressure
- Distance to the closest public transport stop
- Type of public transport available at the closest stop
- Traffic zone in which an accident occurred
- Speed limit of the closest road segment
- Distance to the next road-network intersection
- Distance to the nearest pedestrian crossing
- Proximity and type of bicycle infrastructure
- Public-holiday indicator

This feature engineering process was designed to provide the models with richer contextual information about each accident.

### 3. Exploratory Data Analysis

We used descriptive charts and correlation matrices to understand the structure of the data before modelling.

The analysis included:

- Bar charts comparing accident types with the relative distribution of the target variable
- Correlation matrices for weather features and traffic-volume features
- Visual checks of the geospatial feature engineering process

This step helped us understand which features might be informative for predicting accident severity.

### 4. Model Training and Evaluation

The dataset was split into training, validation, and test sets. The test set was kept untouched until the final evaluation step to approximate the situation of unseen future data.

We trained and compared several classification models:

- Naive Bayes
- Decision tree
- Random forest classifier
- Logistic regression
- Regularised logistic regression using Lasso and Ridge
- Neural networks using Keras

For hyperparameter tuning, we used `RandomizedSearchCV` and `GridSearchCV`. For the neural network, we also experimented with the number of hidden layers and neurons, and later used Keras Tuner for a more systematic search.

## Model Selection

The most important performance metric for our use case was **recall** for the personal-injury class. In an emergency-response context, false negatives are particularly problematic because an accident with personal injury should not be misclassified as property damage only.

Therefore, the model selection focused on reducing false negatives while still maintaining a reasonable overall classification performance.

The strongest models were:

- Random forest classifier
- Logistic regression
- Neural network

The final differences between these models were relatively small. Simpler models such as a basic decision tree and Naive Bayes performed substantially worse on the validation set.

## Results

One of the best-performing models was a random forest classifier. In the final evaluation shown in the project report, the model achieved approximately:

- **Best threshold:** 0.310
- **ROC score:** 0.800
- **Recall:** 0.905

This result reflects the project objective of prioritising the detection of accidents with personal injury. The threshold was selected with this practical use case in mind.

## Key Learnings

This project demonstrated that accident severity prediction requires much more than fitting a classifier to an accident table. The largest part of the work was the construction of a rich modelling dataset.

The main learnings were:

- Feature engineering is central in applied data science projects.
- Geospatial data can add substantial contextual value.
- Time alignment matters when combining accident data with weather and traffic data.
- The choice of evaluation metric should follow the real-world use case.
- In safety-related classification problems, recall can be more important than overall accuracy.
- More complex models do not automatically outperform simpler models.

## Technologies Used

The project was implemented in Python and uses common data science and geospatial libraries, including:

- `pandas`
- `numpy`
- `scikit-learn`
- `geopandas`
- `shapely`
- `matplotlib`
- `seaborn`
- `keras`
- `keras-tuner`

## Authors

This project was created as a group project for the **Workshop Fundamentals of Data Science** at the University of St.Gallen.

Project team:

- Elia Locher
- Marco Molnar
- Gabriel Oget

Supervision:

- Lyudmila Grigoryeva
- Jonathan Chassot
- Hannah Busshoff

## Disclaimer

This project was developed for educational purposes. The results should not be interpreted as a production-ready emergency-response system. Any real-world deployment would require further validation, more robust data pipelines, careful fairness and reliability checks, and close collaboration with domain experts.

