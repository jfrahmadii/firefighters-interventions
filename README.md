# Predicting Firefighter Interventions in High-Risk Car Accident Zones by month in Montreal

## 1. Business Problem

This project addresses the strategic need for the Montreal Fire Department (Service de sécurité incendie de Montréal - SIM) to optimize resource allocation for road-related emergencies. Firefighter interventions are critical at accident scenes for a variety of hazards beyond just fires, including vehicle extrication, medical emergencies, and managing risks from hazardous materials, especially with the rise of Electric and Hybrid Vehicles.

This project aims to build a predictive model to identify high-risk car accident zones that necessitate SIM intervention, enabling data-driven decision-making to enhance incident response, inform prevention efforts, and improve public safety.

## 2. Data Sources

The analysis is based on publicly available data from the [city of Montreal](https://donnees.montreal.ca/), focusing on the years **2012 to 2021**.

* **Firefighter Interventions (SIM)**: This [dataset](https://donnees.montreal.ca/dataset/interventions-service-securite-incendie-montreal) contains records of all interventions carried out by SIM. For this project, it was filtered to include only vehicle-related incidents. 
* **Road Collisions (SPVM)**: This comprehensive [dataset](https://donnees.montreal.ca/dataset/collisions-routieres) contains information on all road accidents, including details on location, time, severity, and contributing factors. A detailed analysis of the missing values in this dataset is provided in the `Missingness_Collisions_Road.ipynb` notebook.

## 3. Methodology

The project follows a systematic data science approach to merge and analyze these datasets to build a predictive model.

### 3.1. Data Preprocessing and Cleaning

1.  **Firefighter Data Consolidation**: The firefighter intervention data from 2005-2014 and 2015-2022 was combined and then filtered to the 2012-2021 period.
2.  **Incident Filtering**: The dataset was filtered to a specific list of 25 vehicle-related incident types (e.g., 'Ac.véh./1R/s.v./ext/29B/D', 'Feu de véhicule extérieur') to isolate relevant events.
3.  **Categorical Data Cleaning**: Borough names (`NOM_ARROND`) were standardized to correct for inconsistencies in the original data.
4.  **Collision Data Cleaning**: Missing values in the road collision dataset were analyzed in a separate notebook. In the main analysis, features with high percentages of missing data were dropped.

### 3.2. Feature Engineering & Data Merging

The main challenge was to link road accidents with firefighter interventions, as they are not directly related in the raw data. Furthermore, the locations (longitude and latitude) of incidents in the interventions dataset were intentionally obfuscated for the privacy reason; therefore, relating incidents to their accidents conterparts was challenging. As the focus has been on high-risk zones defining different zones as the baseline of analysis was targeted. To be able to relate two datasets, boroughs defined as the zones and data from both datasets aggregated on each zone. one of the great advantages of this approch was that missing values were handled by the algorithm without imputations or droping data since all the categorical variables in the original dataset transfer to dummies and each dummy variable represented as new variable for the final dataset including the number of null values. So, in the final dataset, each row shows a borough for a specific year-month and all other values aggregated for that particular spatial and temporal values. 

* **Target Variable Creation**: A binary target variable, **`intervention_risk_level`**, was created. This variable stemed from another defined variable **`firefighter_risk_score`** which was the total number of units deployed to incidents for particular borough and year-month period. the **`intervention_risk_level`** categorized each zone to either 'low-risk' or 'high-risk' by considering the 75th percentile as the threshold for high-risk zones.

### 3.3. Exploratory Data Analysis (EDA)

* **Temporal Analysis**: The number of vehicle-related interventions and accidents per month was analyzed, showing a general trend: while material damaged accidents have been higher during Winter more severe accidents and
deploying more units have accured during Summer and Fall.
* **Spatial Analysis**: A choropleth map was generated to visualize the density of firefighter interventions across Montreal's boroughs, highlighting areas with higher incident concentrations.

### 3.4. Predictive Modeling

The problem was framed as a **binary classification task**: to predict whether a given borough in a specific month is of high-risk interventions.

* **Models Tested**: Several models were trained and evaluated, including:
    * Random Forest
    * LightGBM
    * XGBoost
* **Evaluation Metrics**: The models were compared based on Accuracy, Precision, Recall, and F1-Score with the focus on Recall as it would be important to identify high-risk zones.

## 4. How to Use This Repository


1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/jfrahmadii/firefighters-interventions.git](https://github.com/jfrahmadii/firefighters-interventions.git)
    ```
2.  **Install the dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
3.  **Run the notebooks:**
    * `Montreal_Accidents_Analysis_by_Borough_Intervention_Risk_score.ipynb`: The main notebook containing the full analysis and model development.
    * `Missingness_Collisions_Road.ipynb`: A supplementary notebook with a detailed analysis of missing data.
  
4.  **`data/` folder**: Contains the raw CSV files used in the analysis.

To run the analysis, open and execute the main Jupyter Notebook. Ensure all required libraries listed at the beginning of the notebook are installed.

## 5. Key Findings

To get a better understanding of the results of the model, features that contributed most to the model have been extracted. The following list shows these features.
* **Intersection**
* **Commercial region**
* **Industrial region**
* **Day light**
* **Clear weather**
* **Lane characteristics**

(To understand codes for features inside the model, please check this [link](https://donnees.montreal.ca/dataset/cd722e22-376b-4b89-9bc2-7c7ab317ef6b/resource/f69c656e-9c0c-4763-bb5b-33e5ca46713c/download/rapports-accident-documentation.pdf) out!)

The findings of this analysis suggest that the risk of accidents requiring significant firefighter interventions may not be primarily driven by environmental factors. The predictive model indicates that even under what appear to be ideal weather and road surface conditions, intersections are strongly associated with high-risk events. This suggests that the root cause of these incidents could be less conditional and more structural, pointing to potential challenges in the geometry, traffic flow, and signal management of the intersections themselves. According to the model, accidents at intersections within commercial zones show a particularly strong correlation with events that place a strain on emergency services. Therefore, based on these findings, a targeted engineering review of these specific high-risk locations is recommended. A proactive focus on optimizing signal timing, re-evaluating lane configurations, and enhancing pedestrian safety measures at these key intersections appears to be a promising and potentially high-impact strategy for mitigating severe accidents and reducing the operational load on the fire department

## 6. Future Work

* Incorporate additional datasets, such as weather data ,traffic patterns, and intersection locations, to improve model accuracy.
* Refine the model to predict the **`firefighter_risk_score`** as a predicted required units for a given borough and period.
* Develop a new dataset using approximate spatial analysis to relate incidents and accidents one by one to be able to build more accurate model.


## 7. Contributors


* Jafar Ahmadi 
