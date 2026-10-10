## Spacenomics
### A Bayesian approach to spatial economic forecasting and policy impact analysis.

### Introduction
Spacenomics is a research project investigating the viability of Bayesian inference for economic forecasting using spatial data.
The project explores the relationship between urban street-network structure and local economic productivity, combining spatial network analysis with Bayesian statistical modelling to estimate how changes to the built environment may influence economic outcomes.
Using Cardiff, Wales, as an initial case study, Spacenomics integrates street-network metrics with small-area economic data (LSOA level) to model Gross Value Added (GVA) density. The resulting model is used to explore potential economic shifts under hypothetical changes to the street network.
The ultimate aim is to develop a prediction engine capable of estimating the potential economic implications of proposed spatial interventions.
### Tech Stack
|**Technology**|**Purpose**|
|--------------|-----------|
|GeoPandas|Geospatial data processing, spatial joins and aggregation|
|sDNA|Spatial network analysis and street-network metric calculation
|Shapely|Geometric operations, network cleaning and line splitting
|Pandas|Data processing and economic dataset preparation
|PyMC|Bayesian regression, posterior sampling and probabilistic prediction
|ArviZ|Bayesian inference diagnostics and posterior analysis
|scikit-learn|Spatial clustering for train/test data partitioning
|libpysal|Spatial adjacency weights for neighbourhood-based modelling
|NetworkX|Network data structures and analysis

### Pipeline
The Spacenomics workflow transforms raw street-network and economic data into probabilistic estimates of the potential economic impacts of spatial interventions.
Street Network & Economic Data
              |
              v
       Network Cleaning
       (GeoPandas, Shapely)
              |
              v
  sDNA Calculation & Spatial
         Aggregation
    (sDNA, GeoPandas)
              |
              v
   Data Preparation &
     Standardisation
        (Pandas)
              |
              v
     Bayesian Model
       Estimation
         (PyMC)
              |
              v
    Spatial Validation &
    Posterior Diagnostics
       (ArviZ, PyMC)
              |
              v
   Counterfactual Network
        Modification
              |
              v
 Differential Policy Predictive
           Engine
           (PyMC)
              |
              v
   Spatial Economic Forecasts
       (GeoPandas, GPKG)

       
**1. Network Cleaning**
Tools: GeoPandas, Shapely
Street-network geometries are processed to ensure they match sDNA specifications. This stage includes splitting network lines at junctions and removing duplicate or overlapping street segments.

**2. sDNA Calculation and Spatial Aggregation**
Tools: sDNA, GeoPandas
The cleaned street network is analysed using sDNA to calculate spatial network metrics.
Street-segment metrics are then spatially associated with Lower Layer Super Output Areas (LSOAs) and aggregated to produce area-level explanatory variables.

**3. Data Preparation and Standardisation**
Tools: Pandas, GeoPandas, NumPy
Spatial network metrics are aggregated with economic data to construct the modelling dataset.
The current implementation uses 2023 GVA per square kilometre as its target variable. Predictors and the target are standardised before model fitting.
Spatially clustered train/test partitions are also used to investigate out-of-sample generalisation across different areas.

**4. Bayesian Model Estimation**
Tools: PyMC, libpysal
A Bayesian spatial regression model is used to estimate the relationship between street-network characteristics and local economic productivity.
The model incorporates:
Probabilistic regression coefficients for network metrics.
A Student-t likelihood to accommodate potentially heavy-tailed observations.
Spatially structured random effects using an Intrinsic Conditional Autoregressive (ICAR) prior.
Posterior sampling to represent uncertainty in estimated parameters.
This approach provides a distribution of plausible parameter values and predictions rather than relying solely on point estimates.

**5. Differential Policy Predictive Engine**
Tools: PyMC, GeoPandas, ArviZ
The final stage investigates hypothetical changes to the street network.
A modified network is processed in the same way as the base network (cleaning, sDNA). Differences in the resulting network metrics are then evaluated using posterior samples from the fitted Bayesian model.
The engine produces area-level estimates of:
Expected GVA density change under the model.
Lower and upper uncertainty bounds for the predicted change.
Probability of positive predicted growth for each LSOA.
Forecast outputs are joined to LSOA geometries and exported as a GeoPackage for further spatial analysis and visualisation.

