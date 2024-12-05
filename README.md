# Wildfire Analysis

## Goal of this project
The goal of this work is to explore the impact of wildfires on Arlington, Texas.

Wildfires are uncontrolled fires that burn in wildland areas, including forests, grasslands, and brush. They can occur naturally through lightning strikes or be ignited by human activities, such as campfires, discarded cigarettes, or arson. The primary causes of wildfires include dry weather, high temperatures, low humidity, and strong winds, which create conditions conducive to fire spread. Additionally, the accumulation of combustible materials, such as dead vegetation and forest debris, can significantly increase the likelihood of wildfires. Understanding the various factors that lead to wildfires is crucial for effective prevention and management strategies, especially in regions prone to such incidents.

Analyzing past wildfire data is essential for several reasons, including improving predictive modeling, informing policy decisions, and enhancing public safety measures. Historical data can reveal patterns in wildfire occurrence, helping researchers identify high-risk areas and times when wildfires are more likely to happen. Furthermore, wildfires can have significant implications for air quality, as they release large amounts of particulate matter and other pollutants into the atmosphere. This can lead to deteriorated air quality in nearby urban areas, affecting the health of residents and contributing to environmental issues. By studying past wildfire events, researchers can better understand their impact on air quality and implement measures to mitigate these effects in vulnerable communities.

The project analysis and reflection document can be found in the `/reports` directory.

## Licenses
1. Code used in `compute_distance.py`, `data_clean_aqi.ipynb` and `data_clean_wildfire.ipynb` - This code example extended by the code snippets developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024
2. Accessed data from the US Environmental Protection Agency website which states that you can cite data obtained from the AirData website using the following citation: US Environmental Protection Agency. Air Quality System Data Mart [internet database] available via https://www.epa.gov/outdoor-air-quality-data. Accessed [Oct 30, 2024].
3. Wildland Fires data is provided by the USGS. The data is available under the [USGS Data Policy](https://www.usgs.gov/information-policies-and-instructions/acknowledging-or-crediting-usgs).
4. Accessed data from the CDC WONDER website which states that the data is provided for statistical reporting and analysis only, and allows usage for non-profit and open-source analysis. More on this [here](https://wonder.cdc.gov/datause.html).
5. Data from the Texas Department of State Health Services is provided with permission to reproduce materials as long as the source is cited, including the access date and publication date. More on this [here](https://healthdata.dshs.texas.gov/policy/datause).

## Source data
1. USGS Wildfire Dataset - U.S. Geological Survey (USGS) focuses on the 2021 National Land Cover Database (NLCD), which provides detailed information on land cover and land use patterns across the United States. This particular [dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) (GeoJSON_files.zip - 3.84 GB) has a different spatial scale, spatial resolution, and time period for their particular wildland fire dataset. The purpose of these new datasets is to combine these disparate wildfire datasets, using a common set of attributes, into a single set of polygons with a single fire boundary for each fire. This dataset is intended to create a more comprehensive fire dataset than the existing datasets while eliminating duplication of fire polygons and attributes. The fields that we will be making use of are - 
    * geometryType: Defines the type of shape used for the spatial data.
    * spatialReference: Specifies the coordinate system applied to the geospatial data.
    * fields: A list of dictionaries detailing the name, type, and alias of each of the 30 attributes in the dataset.
    * features: A collection of observations stored in JSON format, with each observation represented as a dictionary containing keys for the attributes (fields) and geometry.
2. [EPS Air Quality System API](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata) - The dataset is sourced from the U.S. Environmental Protection Agency's (EPA) Air Quality System (AQS) API, which provides historical air quality measurements across the United States. While not offering real-time data, this comprehensive database began standardized monitoring with quality assurance procedures in the 1980s, following the EPA's establishment in the early 1970s. The data collection typically initiated between 1983-1988 for most counties, though coverage varies geographically as some regions still lack monitoring stations. For doubts, refer the [FAQ page](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata).
Note - you will require authentication / signup to access this API. More is mentioned in `data_clean_aqi.ipynb`. For rate limits, [refer here](https://www.epa.gov/outdoor-air-quality-data/frequent-questions-about-airdata). The fields that we will be making use of are - 
    * Date (Date): The date of air quality measurement.
    * State/County/City FIPS Codes (String): Numeric codes representing the geographic location of the measurements.
    * AQI Value (Integer): The calculated AQI score for the given date and location.
    * Latitude/Longitude (Float, Float): Coordinates of the monitoring station.
    * Pollutant (String): The specific pollutant measured, such as PM2.5 or Ozone.
    * Station ID (String): A unique identifier for each monitoring station.

3. [CDC Wonder](https://wonder.cdc.gov/): A resource for understanding respiratory disease-related deaths. It provides access to a variety of health-related data, allowing users to analyze trends and causes of respiratory diseases. The Multiple Cause of Death data available on WONDER are county-level national mortality and population data spanning the years 1999-2022. Data are based on death certicates for U.S. residents. Each death certicate contains a single underlying cause of death, up to twenty additional multiple causes, and demographic data. The number of deaths, crude death rates, age-adjusted death rates, and 95% condence intervals for death rates can be obtained by cause of death.
    * Year
    * County - Tarrant County (the county Arlington is in)
    * Gender - to see if there is any dierence between the gender demographics
    * Ten-year age groups - to see if there is any dierence between the ten-year age groups demographics
    * Deaths (due to diseases of the respiratory system) - count of the deaths caused due to issues with respiratory system. These consider “underlying cause of death” only, and will look into the “multiple cause of death” eld during analysis to see if there’s any signicant dierence.

4. [Texas Poison Center Network](https://healthdata.dshs.texas.gov/dashboard/environmental-health/carbon-monoxide-related-poison-calls): Texas Poison Center Network (TPCN) was created in 1993 with the goal of providing 24-hour access to free immediate medical advice for Texans. Anyone can access these services by calling a 24-hour, toll-free hotline, any day of the year. These centers help provide the public, hospitals, emergency responders, and health care providers with information and treatment for poisonous substances, hazardous substances, and similar issues. We collect the aggregated TPCN data from their public dashboard for Carbon Monoxide exposures. This is a dashboard that shows the total number of calls to the TPCN about exposures to carbon monoxide in Texas by year, county, and based on demographic information. We will be manually entering the information that we require from the dashboard, as the download option only allows exporting the visualizations and dashboards as a whole. We will make use of the following elds for now (might include
more during analysis) -
    * Year
    * County - Tarrant County (the county Arlington is in)
    * Rate estimate for the number of CO exposure calls - this will be an estimate based
on the total population of the county

## Intermediate Data Files
Each notebook creates some intermediate files that are used by other analysis notebooks. They are located in `intermediate/` folder. Below is a list of the generated intermediary files:
1. `df_aqi_yearly_weighted.csv` - Contains the yearly AQI estimates for our city.
Note - this is not present in the repository because GitHub limits the size of the dataset that can be uploaded in a repo. Use the codebooks to generate this file (refer methodology).
```
state_code: string, the code representing the state.
county_code: string, the code representing the county.
site_number: string, the unique number identifying the monitoring site.
parameter_code: string, the code representing the pollutant parameter.
poc: integer, the type of sampling point (1 for primary, 2 for secondary, etc.).
latitude: float, the latitude coordinate of the site.
longitude: float, the longitude coordinate of the site.
datum: string, the geographic coordinate system used (e.g., WGS84).
parameter: string, the name of the air quality parameter being measured.
sample_duration_code: string, a code representing the sample duration type.
sample_duration: string, the duration of the sample (e.g., "24 HOUR").
pollutant_standard: string, the standard for the pollutant (e.g., PM10 24-hour standard).
date_local: string, the local date of the measurement.
units_of_measure: string, the units used to measure the pollutant (e.g., Micrograms/cubic meter).
event_type: string, the type of event (e.g., "No Events").
observation_count: integer, the number of observations recorded.
observation_percent: float, the percentage of valid observations.
validity_indicator: string, a validity indicator (e.g., "Y" for valid).
arithmetic_mean: float, the arithmetic mean value of the pollutant.
first_max_value: float, the first maximum value of the pollutant observed.
first_max_hour: integer, the hour during which the first maximum value was observed.
aqi: integer, the Air Quality Index value.
method_code: string, the code representing the method used for measurement.
method: string, the detailed name of the method used for measurement.
local_site_name: string, the name of the local monitoring site.
county: string, the name of the county where the site is located.
city: string, the name of the city where the site is located.
cbsa_code: string, the code representing the Core Based Statistical Area (CBSA).
cbsa: string, the name of the Core Based Statistical Area (e.g., "Dallas-Fort Worth-Arlington, TX").
date_of_last_change: string, the date of the last change to the data record.
```
2. `fire_distance_info.json` - Contains the infomation about how far each  wildfire is from the city.
```
year: integer, represents the year of the data entry.
name: string, the name of the location associated with the data.
size: float, the size of the area affected (in appropriate units).
type: string, the type of event, e.g., "Wildfire."
distance_shortest: list, contains two elements:
first element: float, the shortest distance from the specified location.
second element: list of two floats, the coordinates (latitude and longitude) of the shortest distance point.
avg_distance: float, the average distance from the specified location.
perimeter_start: list of two floats, the coordinates (latitude and longitude) representing the start point of the perimeter.
```
3. `fire_distance_errors.json` - Contains the list of wildfires for which the distance function did't work out for various reasons.
```
fire_id: string, unique identifier for each fire event.
fire_name: string, the name or description of the fire or burn event.
value: string, a value associated with the fire event, in this case, it is always "0".
```
4. `final_smoke_estimates.csv` - Contains our smoke estimates for present years and forecasted years.
```
Year: integer, represents the year of the smoke estimate data.
Smoke_Estimate: float, the estimated value of smoke for that year.
```



## Workflow
1. Download the original wildfire data from [USGS Wildfire Dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and store it in `raw_data` folder.
2. Compute the distances between your desired city and the wildfires using `data_clean_wildfire.ipynb`. Make sure to modify the city parameters accordingly, and also look at theh Gotchas section for any issues with the code involving `multiprocessing` library. This will generate two intermediary files `fire_distance_info.json` and `fire_distance_errors.json`.
3. Get the AQI values for your city using the code in `data_clean_aqi.ipynb`. This will generate one intermediary files `df_aqi_yearly_weighted.csv`. This step might require manual intervention, as this step will change significantly based on the data that you get in previous steps.
4. Run the `smoke_estimates_and_modelling.ipynb` to do the smoke estimates and perform the modelling part. This step will require manual intervention, as this step will change significantly based on the data that you get in previous steps.
5. Run the `extension_data_clean.ipynb` to find out the health impact of the wildfire smokes. Note - this will make use of the healt related datasets in the `\intermediary` path in the repo, so make sure that those files exist. Also, the considerations for this notebook's analysis are pertinent to the city of Arlington, Texas, so the analysis will differ significantly if extended to other cities. Care must be taken.

## About the time-series model - ARIMAX
Our model predicts takes in how smoke levels have changed over time and predicts what they might look like in the future. While this output seems to be satisfactory, it is not guaranteed to hold in real life where multiple confounding variables are influencing the real-life values, especially if there are big changes like climate change.

We start off by fitting an unoptimized ARIMA model and checking the fit coefficients, covariance and metrics to see how well it will perform. We do this on the entire dataset just to test out the waters.

The ARIMA model acronym stands for “Auto-Regressive Integrated Moving Average”. In more details -
* A is the autoregressive component of the ARIMA model, and is represented by AR(p), with the p parameter determining the number of lagged series that we use.
* MA(q) is the moving average model and q is the number of lagged forecasting error terms in the prediction. In an MA(1) model, our forecast is a constant term plus the previous white noise term times a multiplier, added with the current white noise term.

ARIMAX was chosen for forecasting in this analysis because it is well-suited for time series data where past values and external factors influence future outcomes. Unlike linear regression, which assumes a direct and static relationship between variables, ARIMAX accounts for temporal dependencies by considering patterns such as trends, seasonality, and autocorrelation in the data. This is critical for accurately predicting how wildfire smoke impacts health metrics over time, as these relationships often evolve and depend on historical data.

Additionally, ARIMAX incorporates exogenous variables—factors outside the time series itself—to improve predictions. In our case, variables like smoke estimates, average wildfire distance, and GIS-derived burn areas act as predictors that influence air quality and health outcomes. While other models like machine learning-based regressions or exponential smoothing methods could be used, they either require significantly larger datasets or fail to capture the nuanced time-dependent effects as effectively as ARIMAX. Therefore, ARIMAX provides a balance of interpretability and predictive power, making it an ideal choice for this scenario where both historical trends and external variables drive the forecast.


## Gotchas
1. The US EPA was only created in 1973, and did not really begin installing air quality monitoring stations until the early 1980s. Further, of 3000+ counties in the US, the EPA has vetted monitoring stations in only 2000 of them. This means that US EPA AQI measures for any one city will need to be some kind of estimate based on monitoring stations that are nearby. While we took enough precautions to make sure this doesn't skew our analysis and predictions, this is one thing to remember.
2. Calculating AQI is a highly complex process involving multiple calculations, and care has been taken to replicate the same level of accuracy for our analysis wherever possible. For more details on how AQI is calculated, refer [this article](https://www.airnow.gov/aqi/aqi-basics/).
3. Calculating the distance between the city and the wildfires takes a lot of time, so I have optimized the code so that it can run parallely on multiple threads. This might not be a cross-compatible solution, so might have to make suitable changes to run it in different systems.
4. Our ARIMAX model predicts takes in how smoke levels have changed over time and predicts what they might look like in the future. While this output seems to be satisfactory, it is not guaranteed to hold in real life where multiple confounding variables are influencing the real-life values, especially if there are big changes like climate change.

## Limitations
Careful background work and research were done to ensure that there is no extrapolation of the results beyond what the data was indicating. Despite this, this analysis of wildfire impacts on health faces several limitations. A key challenge is the influence of confounding variables, such as changing weather patterns, industrial pollution, and socioeconomic factors, which independently affect air quality and health outcomes. For example, while wildfire smoke is a significant contributor to air quality degradation, other sources like pollution emissions from everyday city activities and indoor pollutants complicate the impact of health outcomes directly to wildfires. These confounding factors introduce uncertainty into the analysis, making it difficult to come to definitive causal relationships between wildfire activity and the above-mentioned deaths and poisonings.

One unique limitation specific to Arlington, Texas, is the abnormal rise in CO poisoning since 2016. The abnormal increase in CO poisoning post-2016 needed looking into, so a lot of external background research was done to understand if this was due to some external factors that go beyond the wildfires. The research led us to a definitive reason behind the increase in CO poisoning. This increase was due to harsher winters, which led to more widespread use of unsafe indoor heating methods, such as burning charcoal or using unventilated heaters. What further increased this issue to its abnormal increase and peaks is the lack of a city or state mandate requiring smoke alarms in homes, as it always is warm in Texas, even during winters. Without this critical safety measure, CO poisoning incidents started going undetected until they became severe, resulting in higher poisoning rates. This unique situation highlighted how different environmental and human factors complicate the analysis where we set out to find the health impacts of wildfire smoke. This strongly emphasizes the need for policy interventions, such as mandating smoke alarms and public safety education campaigns.

Data quality issues also present a significant limitation to the study. Wildfire data may suffer from inaccuracies, such as the misclassification of prescribed fires as wildfires or the underreporting of smaller fire events, which can distort exposure estimates. Similarly, health data, including respiratory disease-related deaths and CO poisoning records, may contain inconsistencies due to underreporting, incomplete datasets, or reliance on secondary sources. These limitations introduce biases and gaps in the data, reducing the reliability of the findings and the ability to establish precise linkages between wildfire activity, air quality, and health outcomes.

Finally, the scope of this study is limited by its focus on a subset of contributing factors and specific health outcomes. While it provides valuable insights into the relationship between wildfires, air quality, and health, it does not account for indirect impacts, such as economic losses, psychological stress, or long-term chronic illnesses. Addressing these limitations would require more comprehensive data collection methods, including integrating environmental, health, and socioeconomic datasets, and employing advanced statistical techniques to account for confounding factors. Future studies should also explore broader impacts to better inform holistic policy responses and community resilience strategies.
