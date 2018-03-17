# Climate analysis using SQLAlchemy ORM & Flask routes for data endpoints

## Data Preparation

Climate data for Hawaii was provided in two CSV files. The content of these files was scrubbed. 

* Jupyter Notebook file called `data_engineering.ipynb` covers data engineering tasks.
* Pandas dataframes are created from the measurement and station CSV files. NaNs and missing values are then cleaned from the data.
* Cleaned CSV files are saved.

## Database Engineering

Using SQLAlchemy to model database schema, sqlite tables for "measurements" and "stations" are created.

* Jupyter Notebook `database_engineering.ipynb` used for database engineering work.
* Pandas used to read cleaned measurements and stations CSV data.
* Database called `hawaii.sqlite` created, using `declarative_base` to create ORM classes for each table, and used `create_all` to populate database.

## Climate Analysis

Climate analysis and data exploration on new weather tables. The following analysis was completed using SQLAlchemy ORM queries, Pandas, and Matplotlib.

* Jupyter Notebook file called `climate_analysis.ipynb` used to complete climate analysis and data exporation.
* Start date and end date determine "vacation" range. 
* Used SQLAlchemy `create_engine` to connect to sqlite database, and `automap_base()` to reflect tables into classes. Referenced those classes as `Station` and `Measurement`.

### Precipitation analysis

* Queries retrieve the last 12 months of precipitation data, and results are plotted using matplotlib
* Pandas dataframes house the summary statistics for the precipitation data.

### Station analysis

* Calculations of the total number of stations, and most active stations.
* Retrieval of the last 12 months of temperature observation data (tobs), filtered by the station with the highest number of observations.
* Plotted results as a histogram with `bins=12`.

### Temperature analysis

* Function `calc_temps` accepts a start date and end date in the format `%Y-%m-%d`, returns the minimum, average, and maximum temperatures for that range of dates.
* Function calculates min, avg, and max temperatures for trip using the matching dates from the previous year (i.e. use "2017-01-01" if trip start date was "2018-01-01")
* Plotted the min, avg, and max temperature from previous query as a bar chart.
  * Used the average temperature as the bar height.
  * Used the peak-to-peak (tmax-tmin) value as the y error bar (yerr).

## Flask Web Application

Flask web app with routes (endpoints) displaying JSON data results from each of the above queries.

### Routes (API endpoints)

* `/api/v1.0/precipitation`

  * Queries dates and temperature observations from the last year.
  * Converts query results to a dictionary using `date` as the key and `tobs` as the value.
  * Returns the json representation of dictionary.

* `/api/v1.0/stations`
  * Returns a json list of stations from the dataset.

* `/api/v1.0/tobs`
  * Returns a json list of temperature observations (tobs) for the previous year

* `/api/v1.0/<start>` and `/api/v1.0/<start>/<end>`

  * Returns a json list of the minimum temperature, the average temperature, and the max temperature for a given start or start-end range.
  * Given the start only, calculates `TMIN`, `TAVG`, and `TMAX` for all dates greater than and equal to the start date.
  * Given the start and the end date, calculates the `TMIN`, `TAVG`, and `TMAX` for dates between the start and end date inclusive.
