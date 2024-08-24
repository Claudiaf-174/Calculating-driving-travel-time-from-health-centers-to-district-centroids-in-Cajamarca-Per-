# README: Data Analysis of Health Establishments in Cajamarca Using Google Maps API

## Project Overview
This project focuses on analyzing the proximity and travel times from district centroids in the Cajamarca region to health establishments. The analysis includes calculating distances using the Google Maps API, and visualizing data in various ways (e.g., travel time histograms, distance histograms, etc.). 

The key steps of the project include:
1. Importing and cleaning datasets related to health establishments and district centroids.
2. Calculating travel distances and times using Google Maps API.
3. Visualizing the data using histograms and bar plots.

## Prerequisites
Make sure you have the following installed and properly set up:
- Python 3.x
- Google Maps API key
- Required libraries (can be installed using `pip`):

### Installing Required Libraries
```bash
pip install googlemaps pandas matplotlib seaborn numpy
```

The key Python libraries used include:
- `pandas`: for data manipulation.
- `numpy`: for mathematical operations.
- `googlemaps`: for accessing the Google Maps Directions API.
- `matplotlib` and `seaborn`: for data visualization.

### Google Maps API Setup
You need a valid Google Maps API key for this project. You can create one by following the steps [here](https://developers.google.com/maps/documentation/directions/get-api-key).

## Dataset Overview

### Datasets
1. **Health Establishments Data**: Includes the location and details of health establishments in the Cajamarca region.
   - File: `cajamarca_data_health_estb.xlsx`
   - Columns include `id_eess`, `nombre`, `direccion`, `latitud`, `longitud`, etc.

2. **District Centroids Data**: Provides centroid coordinates of districts in Peru.
   - File: `peru_districts_centroids.xlsx`
   - Columns include `NOMBDEP` (department name), `NOMBDIST` (district name), `Centroid_Latitude`, `Centroid_Longitude`, etc.

### Loading Datasets
The datasets are loaded using the following code:
```python
import pandas as pd

# Load the datasets
health_estb_df = pd.read_excel('cajamarca_data_health_estb.xlsx')
districts_centroids_df = pd.read_excel('peru_districts_centroids.xlsx')

# Filter for Cajamarca district centroids
cajamarca_districts = districts_centroids_df[districts_centroids_df['NOMBDEP'] == 'CAJAMARCA']
```

## Steps

### 1. Calculating Euclidean Distances
A function was created to calculate the Euclidean distance between district centroids and health establishments:

```python
def euclidean_distance(lat1, lon1, lat2, lon2):
    return np.sqrt((lat2 - lat1)**2 + (lon2 - lon1)**2)
```

This function was applied to identify the closest health establishment for each district in the Cajamarca region.

### 2. Using Google Maps API to Calculate Travel Time and Distance
We used the Google Maps Directions API to calculate the travel time and distance between health establishments and district centroids:

```python
gmaps = googlemaps.Client(key='YOUR_GOOGLE_API_KEY')

# Calculate driving time and distance between coordinates
for i, row in merged_df.iterrows():
    directions_result = gmaps.directions(row['HE_coord'], row['District_coord'], mode="driving")
```

### 3. Data Cleaning and Merging
After calculating the distances and times, the results were merged into a single DataFrame for further analysis.

```python
# Merging dataframes for combined analysis
merged_df = pd.merge(health_estb_df, closest_health_estb_df, on='CODIGO', how='inner')
```

### 4. Visualizing the Data

#### Travel Time Histograms
Histograms were created to visualize the distribution of travel times across different traffic models (best_guess, pessimistic, optimistic):

```python
sns.histplot(data=result_df, x='min', hue='traffic_model', multiple="stack")
```

#### Travel Distance Histograms
Similarly, travel distance histograms were created to show the distribution of travel distances for each traffic model:

```python
sns.histplot(data=df_long, x='distance', hue='traffic_model', multiple="stack")
```

#### Mean Travel Time and Distance Bar Plots
We calculated and plotted the mean travel times and distances for each province, colored by traffic model:

```python
sns.barplot(x='NOMBPROV', y='time', hue='traffic_model', data=mean_time_result)
```

## Visualizations
Key visualizations include:
1. **Histograms**: Travel time and distance distributions by traffic model.
2. **Bar Plots**: Mean travel time and distance by province.

### Example of Bar Plot Code:
```python
# Plot the mean travel distance by province
plt.figure(figsize=(10, 6))
sns.barplot(x='NOMBPROV', y='distance', hue='traffic_model', data=mean_distance_original)
plt.title('Mean Travel Distance by Province')
plt.show()
```

 
