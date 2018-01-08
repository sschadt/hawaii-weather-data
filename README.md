# Climate analysis using SQLAlchemy ORM & Flask API for corresponding data endpoints

## 1 - Data Engineering

The climate data for Hawaii was provided in two CSV files. The content of these files was scrubbed. 

* Jupyter Notebook file called `data_engineering.ipynb` used to complete all Data Engineering tasks.
* Pandas read the measurement and station CSV files as DataFrames.
* Cleaned data from NaNs and missing values. 
* Saved cleaned CSV files.


## 2 - Database Engineering

Use SQLAlchemy to model  table schemas and create a sqlite database for  tables - one table for measurements and one for stations.

* Jupyter Notebook called `database_engineering.ipynb` used to complete all of  Database Engineering work.
* Pandas used to read cleaned measurements and stations CSV data.
* Created a database called `hawaii.sqlite`.
* Used `declarative_base` to create ORM classes for each table.
* Create the tables in the database using `create_all`.


## 3 - Climate Analysis

Climate analysis and data exploration on new weather station tables. All of the following analysis was completed using SQLAlchemy ORM queries, Pandas, and Matplotlib.

* Created Jupyter Notebook file called `climate_analysis.ipynb`, used to complete climate analysis and data exporation.
* Start date and end date determine vacation range - approximately 3-15 days total.
* Used SQLAlchemy `create_engine` to connect to sqlite database.
* Used SQLAlchemy `automap_base()` to reflect tables into classes, and saved a reference to those classes called `Station` and `Measurement`.

### Precipitation Analysis

* Queried to retrieve the last 12 months of precipitation data
* Plotted results
* Used Pandas to print the summary statistics for the precipitation data.

### Station Analysis

* Calculate the total number of stations.
* Determine the most active stations.

  * List the stations and observation counts in descending order
  * Determine the station with the highest number of observations

* Retrieve the last 12 months of temperature observation data (tobs).

  * Filtered by the station with the highest number of observations.
  * Plotted the results as a histogram with `bins=12`.

### Temperature Analysis

* Function `calc_temps` accepts a start date and end date in the format `%Y-%m-%d`, returns the minimum, average, and maximum temperatures for that range of dates.

* Above function calculates min, avg, and max temperatures for trip using the matching dates from the previous year (i.e. use "2017-01-01" if trip start date was "2018-01-01")

* Plotted the min, avg, and max temperature from your previous query as a bar chart.

  * Used the average temperature as the bar height.
  * Used the peak-to-peak (tmax-tmin) value as the y error bar (yerr).

## 4 - Flask App

Flask API based on above queries.

### Routes

* `/api/v1.0/precipitation`

  * Queries dates and temperature observations from the last year.
  * Converts query results to a Dictionary using `date` as the key and `tobs` as the value.
  * Returns the json representation of dictionary.

* `/api/v1.0/stations`
  * Returns a json list of stations from the dataset.

* `/api/v1.0/tobs`
  * Returns a json list of Temperature Observations (tobs) for the previous year

* `/api/v1.0/<start>` and `/api/v1.0/<start>/<end>`

  * Returns a json list of the minimum temperature, the average temperature, and the max temperature for a given start or start-end range.
  * Given the start only, calculates `TMIN`, `TAVG`, and `TMAX` for all dates greater than and equal to the start date.
  * Given the start and the end date, calculates the `TMIN`, `TAVG`, and `TMAX` for dates between the start and end date inclusive.
