# CityBikes-API-Project

## Project/Goals

The goal of this project was to pull bike station data for a specific city from [citybik.es](https://citybik.es) and then using the bike station data perform a lookup for venues within a 1KM radius. Then utilizing both these datasets we were able to create visualizations and regression models using various Python libraries.

## Process

**Importing Data from Various APIs**
- Started with [citybik.es](https://citybik.es)
  - Very simple to get set up
  - Loaded data directly into a DataFrame
- Moved on to Foursquare & Yelp (processes were similar)
  - Unlike citybikes, had to set up authorization and proper headers
  - Iterated through the bike station list to call Foursquare for various POIs within the set radius (1KM for this project)
  - Appended API results and station_id to a list so I could tie each station to a POI
  
**Cleaning Data**
  - After pulling data from the API I noticed the location and categories were still in JSON format, making them unpleasant to read or analyze. So, I created various data cleaning functions and used Panda's `.apply()` function to extract the important values into new columns before dropping the old ones. This resulted in much cleaner data exports
  - Renamed various columns to make them less ambiguous when joining with other tables later
  - Created a custom de-duplication function that looks for rows with identical names & addresses and then only keeps the row with the shortest distance from the station

**Visualizing Data**
- Used `.merge()` to combine the two previously exported '.csv's to create a master table for use in the visualizations
- Decided on the use of PyData's [seaborn](https://seaborn.pydata.org/) to visualize my data due to the ease of use, visually pleasing graphs and extensive customizability
- Visualizations and brief summaries of my findings can be found in `notebooks/joining_data.ipynb`
  
**Regression Model**
- Created regression models utilizing the statsmodel API to further understand the relationship between various variables
- Found a few relationships that made logical sense (e.g. places with lower ratings were farther away from bike stations and thus farther away from the city) however in most cases the R-squared value was too low to make any meaningful assertions

## Results

- For my chosen city, I went with Yelp as my chosen API. Not only did they have more places located near my chosen bike stations, but the data was much more complete, providing additional columns to perform analysis on.
  
- Created some visualizations in `notebooks/joining_data.ipynb` that provide an insight into the joined datasets, however in most cases, it is hard to paint a direct relation between values.

- I utilized various data points when creating my linear regression models however it was hard to find two data points that shared a high enough R-squared value to be considered as the defining relationship between the located bike stations and chosen Yelp POIs.

## Challenges 

- Parsing the data from the API into the Pandas DataFrame was the biggest hurdle to overcome for me. I had written out most of the code for the `notebooks/yelp_foursquare_EDA.ipynb` notebook when I realized that the station ID needed to be included in the API result DataFrame. This meant I had to go back and rewrite a lot of my previous work to account for this.

- I found the lack of a "Restauraunt" category in the Yelp API kind of frustrating. They had subcategories (e.g. Scottish Food), but a general tag to encompass all of them would have been nice for analysis reasons

- The tight limitation of the Yelp API meant that I had to consider how often I could query the API. Resulting in me making multiple API keys to compensate.

## Future Goals

- If I had more time available and more API access I would try to query various other cities with larger data sets. I picked Glasgow because it is close to where I was born however there was a limited amount of stations & venues for me to perform analysis on.

- I also would have liked to see if I could normalize the SQLite database further by breaking down the tables by Venue_ID so my model could have tolerated a single venue for multiple bike stations. I had to limit it to one venue per bike station by choosing the shortest route and dropping the rest, which could affect my visualizations