# 🎧 Listening to the Weather: Modelling for a Weather-Based Spotify Playlist
## 🌦 In this project, I built a model that can curate a Spotify playlist based on the day's weather. The model uses daily data on weather and the Top 50 songs on Spotify in Tokyo and Singapore between 18 October 2023 and 30 April 2025.

Hello! This serves as my capstone project for General Assembly's Data Analytics Bootcamp. 

### 🎯 Project Aims
The project aims to address the following problems:<br/>

a) To provide a solution for Spotify users who want to listen to music suitable for that day's weather (there are multiple studies that have proven that people's music listening habits are affected by weather conditions to some extent).<br/>
b) To provide Spotify with a novel new marketing idea - even if short-term - to boost user engagement in an increasingly competitive music streaming market in Asia.

### 🇯🇵 Why Tokyo? (And Why Was a Portion of the Project Dedicated to Singapore's Context?)
There are two main reasons why I chose to use Tokyo as the point of focus for this project:<br/>

a) I wanted to test the effectiveness of the model using a territory that had greater seasonal variation (i.e. it had distinct seasons, each with different general weather conditions)<br/>
b) In my preliminary research, I found that Spotify has never executed a promotional campaign that tapped on weather changes in Asia. I wanted to see if this was feasible – even if as a short-term, hype-building strategy – in Asia. 

Later on in the code, I tried replicating a similar approach to modelling using Singapore's data, just to see how feasible my idea would be in Singapore's context. It must be noted that there are marked differences between Tokyo's and Singapore's weather:<br/>

a) Singapore does not have different prefecture- or state-like geographies.<br/>
b) Singapore also does not experience a temperate climate marked by 4 distinct seasons.

### 🙋‍♂️ Will the Model Recommend a Specific Song Per Day, Based on That Day's Weather?
**No.** This is because I found this approach too limiting to cater to the variety of ways people may change their music listening habits according to the weather. 

For example, on a cold and rainy winter day, some people may want to listen to slower, more mellow music to match the similarly downcast weather. However, others may want to listen to more upbeat, energetic music to maintain their focus throughout the working day under the same conditions

### 🤔 So What will the Model Recommend?
Instead of recommending a single song per day, I will make the model curate a whole playlist based on each day's weather.

This will be done by first creating a list of distinct songs that have ever shown up in Tokyo's daily Spotify Top 50 within the specified time range, conducting hierarchical clustering on this list, and grouping the daily Spotify Top 50 data by date and cluster. 

This will give us a new dataframe where for every index, besides the 'Date' column, each column represents the proportion of songs from each cluster that appeared in that day's Top 50. 

The new dataframe will then be combined with each day's weather data, before being sent for predictive modelling. 

### 👨‍💻 Data Used
#### Weather Data
Weather Data was sourced from [Open-Meteo](https://open-meteo.com/en/docs/historical-weather-api), and includes the following daily measurements:<br/>

a) Min, Max & Mean Temperature (°C)<br/>
b) Rain Sum (mm), Snowfall Sum (cm), Precipitation [rain, showers and snow] Sum (mm)<br/>
c) Min, Max & Mean Relative Humidity [recorded at 2m above ground] (%)<br/>
d) Min, Max & Mean Wind Speed [recorded at 2m above ground] (km/h)<br/>
e) Daylight Duration (s)<br/>
f) Sunrise & Sunset Times (iso8601)<br/>

*Note: For each day, I also added one-hot encoded columns **'season_Spring'**, **'season_Summer'** and **'season_Winter'** to indicate which season the day belonged to.*

#### Spotify Data
Spotify Data was taken from [Kaggle user asaniczka's 'Top Spotify Songs in 73 Countries' dataset](https://www.kaggle.com/datasets/asaniczka/top-spotify-songs-in-73-countries-daily-updated). Each index represents a song from the Top 50 of a specific country on a specific date, but I only used data that was specific to Tokyo (and Singapore). While this data includes a variety of columns which describe the song's metadata (e.g. 'spotify_id', 'popularity', 'duration_ms', etc), I mainly used columns which represent scores Spotify's algorithm has issued to each song for a variety of musical characteristics. These columns are (explanations provided by asaniczka's data dictionary): <br/>

a) Danceabilty: A measure of how suitable the song is for dancing based on various musical elements.<br/>
b) Energy: A measure of the intensity and activity level of the song.<br/>
c) Loudness: The overall loudness of the song in decibels.<br/>
d) Mode: Indicates whether the song is in a major or minor key.<br/>
e) Speechiness: A measure of the presence of spoken words in the song. *(Can indicate whether the song has rapped vocals)*<br/>
f) Acousticness: A measure of the acoustic quality of the song.<br/>
g) Instrumentalness: A measure of the likelihood that the song does not contain vocals.<br/>
h) Valence: A measure of the musical positiveness conveyed by the song.<br/>
i) Tempo: The tempo of the song in beats per minute.<br/>

### 🤖 What columns did the dataset sent for modelling have?
1. snapshot_date - Date of data
2. 1 - Proportion of Day's Top 50 from Song Cluster #1 
3. 2 - Proportion of Day's Top 50 from Song Cluster #2
4. 3 - Proportion of Day's Top 50 from Song Cluster #3
5. 4 - Proportion of Day's Top 50 from Song Cluster #4
6. 5 - Proportion of Day's Top 50 from Song Cluster #5
7. 6 - Proportion of Day's Top 50 from Song Cluster #6
8. 7 - Proportion of Day's Top 50 from Song Cluster #7
9. 8 - Proportion of Day's Top 50 from Song Cluster #8
10. 9 - Proportion of Day's Top 50 from Song Cluster #9
11. temperature_2m_max (°C) – Day's Max Temperature in °C
12. temperature_2m_min (°C) – Day's Min Temperature in °C
13. temperature_2m_mean (°C) – Day's Mean Temperature in °C
14. rain_sum (mm) - Sum of Day's Rainfall in milimeters
15. snowfall_sum (cm) - Sum of Day's Snowfall in centimeters
16. precipitation_sum (mm) – Sum of Day's Precipitation (rain, snow & showers) in milimeters
17. relative_humidity_2m_max (%) – Day's Max Relative Humidity as a Percentage
18. relative_humidity_2m_min (%) – Day's Min Relative Humidity as a Percentage
19. relative_humidity_2m_mean (%) – Day's Mean Relative Humidity as a Percentage
20. wind_speed_10m_max (km/h) - Day's Max Wind Speed in Kilometers per Hour
21. wind_speed_10m_min (km/h) - Day's Min Wind Speed in Kilometers per Hour
22. wind_speed_10m_mean (km/h) - Day's mean Wind Speed in Kilometers per Hour
23. daylight_duration (s) - Number of Seconds of Day's Daylight
24. sunrise_time - Time when Sunrise Occurred, reformatted as a float
25. sunset_time - Time when Sunset Occurred, reformatted as a float
26. season_Spring - One-Hot Encoded column indicating whether day was from Spring
27. season_Summer - One-Hot Encoded column indicating whether day was from Summer
28. season_Winter - One-Hot Encoded column indicating whether day was from Winter
