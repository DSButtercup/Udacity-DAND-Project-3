# Communicate Data Findings: Airline On-Time Performance

## Dataset
### Description
This project investigates the on-time performance for commercial domestic flights in the United States for the year 2006. The data that were used for the project are from the Bureau of Transportation Statistics, as compiled for the 2009 ASA Statistical Computing and Graphics Data Expo. The complete set of data, available [here](https://www.google.com/url?q=http://stat-computing.org/dataexpo/2009/the-data.html&sa=D&ust=1554484977403000), is for the period from 1987 to 2008. In this project, I used data only for 2006. The data for this project are comprised of 4 CSV files:
- `2006.csv.bz2`, 109MB compressed and 640MB uncompressed, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/EPIFFT),
- `airports.csv`, 238KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/XTPZZY),
- `carriers.csv`, 42.7KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/3NOQ6Q),
- `plane-data.csv`, 418KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/YZWKHN).

### Data Wrangling

The majority of the wrangling efforts were directed towards the `flights` dataset. Data cleaning focused on these issues:
- Dates and times which were recorded as integers and floating point numbers in multiple separate columns were combined, formatted as timestamps, and stored in a single column each.
- Missing values were replaced where possible, and entries where missing values would interfere with analysis were dropped from the dataframe.
- For each row, the dataset contained one date across 3 columns (year, month, day), and 4 columns for actual departure time, scheduled departure time, actual arrival time, and scheduled arrival time. This resulted in the following 2 inconsistencies:
    - For some entries, flight delays (or earliness) were such that the scheduled and actual departure times were not on the same day. I corrected this by adjusting the dates for these entries by one day after conversion to timestamps.
    - For some entries, the (scheduled or actual) departure time, was not on the same day as the (scheduled or actual) arrival time, either because of the long duration of the flight, or the difference in timezone between the origin and destination airports. I again corrected for this by adjusting these dates forward or backward by 1 day.
- There were entries for some time columns where the integer values were larger than 2359. These were retained and corrected by subtracting 2400 to get the time, and adding 1 day to the timestamp.
- There were invalid values for some entries, like negative values for flight durations. Such entries were dropped from the dataframe.
- Extreme outliers were assessed and addressed for all columns where it was applicable.

The final wrangled dataset is over 1.18GB and cannot be uploaded to GitHub, but I will be making it public on Kaggle soon and updating this README with the appropriate link.

The other 3 datasets did not require as comprehensive a wrangling exercise. For the `airports` dataset, a few entries that were not located in the USA were removed. Additionally, some missing location values were replaced for several entries. For the `carriers` dataset, a single missing value was replaced. For the `planes` dataset, some data types for dates were corrected to timestamps from strings, and entries with missing values were dropped. The wrangled datasets are included with this project:
- `airports_master.csv`, 205KB,
- `carriers_master.csv`, 35.5KB,
- `planes_master.csv`, 401KB.

Note: If using Pandas in Python, one valid value in the first column of the `carriers.csv` and `carriers_master.csv` files is the string "NA", and with default settings for `pandas.read_csv`, it can read in as a null value instead.

## Summary of Findings

Below are the findings from my exploratory analysis:
- `DepDelay` has a heavy right skew. I applied the log transformation, and the result maintained the right skew. Thus, this variable does not have either a normal or lognormal distribution.

- Similarly to the above, `ArrDelay` has a right skew, with the log transformed distribution also having a right skew. The conclusion was this variable does not follow a normal distribution.

- `OverallDelay` showed a normal distribution, without need for any transformations.

- `Distance` has a right skew when linearly scaled. With a log scale, the distribution was almost symmetrical. The conclusion was that the variable follows a lognormal distribution.

- `ActualElapsedTime` is right skewed. When a log scale is applied, the distribution is more symmetrical, although with a smoother slope on the left than on the right. The conclusion was this distribution is approximately lognormal. CRSElapsedTime showed similar distribution after similar transformation

- I observed that flights scheduled to depart in the late afternoon to early evening saw more delays than on time flights.

- Flights scheduled to arrive at 6 PM or later also showed more delayed flights, while flights earlier in the day had more on time flights.

- Additionally, flight delays tend to happen to similar extents for both longer flights and shorter flights, in terms of duration.

- Expectedly, following from the above, the relationship with flight distance is similar for both groups, since more distant flights are likely to have higher durations. The distribution of distance is almost exactly the same for both gorups.

- There are more scheduled and actual arrivals and departures during the day than during the night.

- Airports which individually have the most arrivals and departures also make up some of the flight routes with the most flights. A flight route, in this context, is a unique pairing of origin airport and destination airport. This is directional. The flight route from airport A to airport B is not the same as the flight route from airport B to airport A.

- More flights are on time than delayed.

- Flights scheduled for nighttime arrivals showed higher durations than those scheduled for daytime arrivals.

- The relationship was the opposite for scheduled flight durations, which showed shorter flight durations.

- Fittingly, both of the above applied to flight distances as well.

## Key Insights for Presentation

The slide deck file is included in this project: `Part_II_slide_deck.slides.html`. It can be opened and viewed in any web browser.

These are the insights included in the presentation:
- More flights are on time than delayed, overall.
- There are more scheduled flights during the day than at night.
- There are more flight delays for flights departing or landing at night than during the day.

The plots for the presentation have larger plot sizes in the slide deck than I was using to merely render them in my notebook.