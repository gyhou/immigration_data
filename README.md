# Immigration & Airport Data

## Project Summary
**Aggregate the immigration data by airport in order to find out the relationship between the number of immigrants and the airports.**

Data stored in S3 bucket `s3a://immigration-airport-data/`

The project follows the follow steps:
1. [Project Scope and Data Source](README.md#Scope-of-project-and-Data-Source)
1. [Explore and Assess the Data](README.md#Explore and Assess the Data)
1. [Data Model](README.md#Data Model)
1. [ETL to Model the Data]
1. [Data Quality Check]
1. Data Dictionary

## Scope-of-project-and-Data-Source

### Scope 
- Aggregate the immigration data by airport in order to find out the relationship between the number of immigrants and the airports.

### Data Source
1. (New) Airport codes data downloaded from https://datahub.io/core/airport-codes
    - 6 different types of airports
    - Location for each type of airport in the area
1. Immigration data from i94 https://travel.trade.gov/research/reports/i94/historical/2016.html
    - 12 files for each month, each with around 3 million rows
    - Contains each immigrant entry port and basic information

### Solution
- Aggregated data partitioned by year and month the total immigrants and number of airports in each city.

### Technical Tools
- Jupyter Notebook (Python): Test code to read/process airport/immigration files and read/write parquet files using PySpark
- Spark: PySpark was used to read and process the data (each month of immigration data has ~3 million rows)
  - Saving the files in S3 by partitioning the parquet files allows for faster processing speed
- AWS: Used S3 to store and save data, easily scalable

## Explore and Assess the Data
### Airport Data
- Aggregate airport types by city

#### Feature Engineer
- Split the region columns to show both country and state separately
- Pivot table to show aggregation of types of airports in each location

#### Clean data
- Remove Null values from column municipality
- Make sure data type fit schema
- Check for null values in Primary Key and Not Null columns

#### Create module to convert i94 immigration codes
- `port_codes_dict`: convert port codes to port names
- `city_res_codes_dict`: convert city & res code to names

### Immigration Data
- Aggregate immigrants by port, city, res

#### Feature Engineer
- Convert all port, city, res codes to names
- Split the port names to city and state

#### Clean data
- Make sure data type fit schema
- Check for null values in Primary Key and Not Null columns

## Data Model
### Fact Table
- `immigration_airport_aggregate`
  - stores the number of immigrants in each port for each month of the year
  - also shows the number of different types of airports for each location

### 4 Dimension Tables
- `airport_types`: shows the location and types of airports in each city
- `airport_aggregate`: stores the number of types of airports in each city
- `immigration_city_res`: stores the number of immigrants coming from a given city
- `immigration_port`: stores the number of immigrants to each port

![](airflow-pipeline.png)

























# Scope the Project and Gather Data
Since the scope of the project will be highly dependent on the data, these two things happen simultaneously. In this step, you’ll:

Identify and gather the data you'll be using for your project (at least two sources and more than 1 million rows). See Project Resources for ideas of what data you can use.
Explain what end use cases you'd like to prepare the data for (e.g., analytics table, app back-end, source-of-truth database, etc.)
# Step 2: Explore and Assess the Data
Explore the data to identify data quality issues, like missing values, duplicate data, etc.
Document steps necessary to clean the data
# Step 3: Define the Data Model
Map out the conceptual data model and explain why you chose that model
List the steps necessary to pipeline the data into the chosen data model
# Step 4: Run ETL to Model the Data
Create the data pipelines and the data model
Include a data dictionary
Run data quality checks to ensure the pipeline ran as expected
Integrity constraints on the relational database (e.g., unique key, data type, etc.)
Unit tests for the scripts to ensure they are doing the right thing
Source/count checks to ensure completeness
# Step 5: Complete Project Write Up
What's the goal? What queries will you want to run? How would Spark or Airflow be incorporated? Why did you choose the model you chose?
Clearly state the rationale for the choice of tools and technologies for the project.
Document the steps of the process.
Propose how often the data should be updated and why.
Post your write-up and final data model in a GitHub repo.
Include a description of how you would approach the problem differently under the following scenarios:
If the data was increased by 100x.
If the pipelines were run on a daily basis by 7am.
If the database needed to be accessed by 100+ people.
