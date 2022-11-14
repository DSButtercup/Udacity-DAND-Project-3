# Bureau of Transportation Statistics Airline On-Time Performance Data Exploration
## <center> by Marissa Mutare </center>


## Dataset
This project investigates the on-time performance for commercial domestic flights in the United States for the year 2006. The data that was used for the project is from the Bureau of Transportation Statistics, as compiled for the 2009 ASA Statistical Computing and Graphics Data Expo. The complete set of data, available [here](https://www.google.com/url?q=http://stat-computing.org/dataexpo/2009/the-data.html&sa=D&ust=1554484977403000), is for the period from 1987 to 2008. In this project, I used data only for 2006. The data for this project is comprised of 4 CSV files:
- `2006.csv.bz2`, 109MB compressed and 640MB uncompressed, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/EPIFFT),
- `airports.csv`, 238KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/XTPZZY),
- `carriers.csv`, 42.7KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/3NOQ6Q),
- `plane-data.csv`, 418KB, available [here](https://dataverse.harvard.edu/api/access/datafile/:persistentId?persistentId=doi:10.7910/DVN/HG7NV7/YZWKHN).

## Data Wrangling

The majority of the wrangling efforts were directed towards the `flights` dataset. Data cleaning focused on these issues:
- Dates and times which were recorded as integers and floating point numbers in multiple separate columns were combined, formatted as timestamps, and stored in a single column each.
- Missing values were replaced were possible, and entries where missing values would interfere with analysis were dropped from the dataframe.
- For each row, the dataset contained one date across 3 columns (year, month, day), and 4 columns for actual departure time, scheduled departure time, actual arrival time, and scheduled arrival time. This resulted in the following 2 inconsistencies:
    - For some entries, flight delays (or earliness) were such that the scheduled and actual departure times were not on the same day. I corrected this by adjusting the dates for these entries by one day after conversion to timestamps.
    - For some entries, the (scheduled or actual) departure time, was not on the same day as the (scheduled or actual) arrival time, either because of the long duration of the flight, or the difference in timezone between the origin and destination airports. I again corrected for this by adjusting these dates forward or backward by 1 day.
- There were entries for some time columns where the integer values were larger than 2359. These were retained and corrected by subtracting 2400 to get the time, and adding 1 day to the timestamp.
- There were invalid values for some entries, like negative values for flight durations. Such entries were dropped from the dataframe.
- Extreme outliers were assessed and addressed for all columns where it was applicable.

The final wrangled dataset is over 1.18GB and cannot be uploaded to GitHub, but I will be making it public on Kaggle soon and updating this README with the appropriate link.

The other 3 datasets did not require as comprehensive a wrangling exercise. For the `airports` dataset, a few entries that were not located in the USA were removed. Additionally, some missing location values were replaced for several entries. For the `carriers` dataset, a single missing value was replaced. For the `planes` datassets, some data types for dates were corrected to ti,estamps from strings, and entries with missing values were dropped. The wrangled datasets are included with this project:
- `airports_master.csv`, 205KB,
- `carriers_master.csv`, 35.5KB,
- `planes_master.csv`, 401KB.

Note: If using Pandas in Python, one valid value in the first column of the `carriers.csv` and `carriers_master.csv` files is the string "NA", and with default settings for `pandas.read_csv`, it can read in as a null value instead.