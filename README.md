# A Serverless Approach to Scalable YouTube Trend Analysis with ETL Automation

## Project Overview
The Serverless Approach to Scalable YouTube Trend Analysis with ETL Automation project is designed to analyze region-wise YouTube data and uncover insights using AWS services. It employs a scalable, serverless architecture for data ingestion, transformation, and visualization, making use of modern cloud technologies to process large datasets efficiently.

## Architecture

The project utilizes the following AWS services:
- *S3*: For storing raw, cleaned, and processed data.
- *AWS Lambda*: To automate .json to Parquet file conversion.
- *AWS Glue*: For data transformation and schema management.
- *Amazon Athena*: To query processed data using SQL.
- *Amazon QuickSight*: To create interactive dashboards and visualize trends.

### Workflow

1. *Data Ingestion*  
   Raw YouTube data in .csv format (region-wise) and reference data in .json format are uploaded to the *S3 bucket* youtube-trend-analysis-raw-useast1-dev.

2. *Lambda Function*  
   A Lambda function, youtube-trend-analysis-raw-useast1-lambda-json-parquet, triggers on .json file uploads. It converts .json files to Parquet format and stores them in the S3 bucket youtube-trend-analysis-cleaned-useast1-dev.

3. *ETL Job 1*  
   An AWS Glue job, youtube-trend-analysis-cleaned-csv-to-parquet, pulls .csv files from the raw bucket, transforms them to Parquet format, and stores them in the cleaned bucket.

4. *ETL Job 2*  
   Another AWS Glue job, youtube-trend-analysis-parquet-analytics-version, joins raw data with reference data (using category_id as a foreign key). The final processed data is stored in the *S3 bucket* youtube-trend-analysis-useast1-dev.

5. *Schema Creation*  
   Glue crawlers generate schemas for all datasets in respective Glue databases:
   - youtube_trend_analysis_raw
   - db_youtube_cleaned
   - db_youtube_analytics

6. *Data Querying*  
   Processed data is queried using *Amazon Athena* for analysis and validation.

7. *Visualization*  
   *Amazon QuickSight* connects to db_youtube_analytics and visualizes trends through dashboards, offering insights into regional YouTube activity.
   
## AWS Services

### S3 Buckets
- youtube-trend-analysis-raw-useast1-dev: Stores raw .csv and .json data.
- youtube-trend-analysis-cleaned-useast1-dev: Stores cleaned data in Parquet format.
- youtube-trend-analysis-useast1-dev: Stores final joined and transformed data.

### AWS Lambda
- youtube-trend-analysis-raw-useast1-lambda-json-parquet: Converts .json to Parquet upon file upload.

### AWS Glue
- *ETL Jobs*
  - youtube-trend-analysis-cleaned-csv-to-parquet: Cleans raw .csv files.
  - youtube-trend-analysis-parquet-analytics-version: Joins raw and reference data.
- *Databases and Tables*
  - youtube_trend_analysis_raw: Contains raw data tables.
  - db_youtube_cleaned: Contains cleaned data tables.
  - db_youtube_analytics: Stores processed data for querying and visualization.

### Amazon Athena
- Queries processed data directly from Glue databases for analysis.

### Amazon QuickSight
- Visualizes analytical data using dashboards connected to db_youtube_analytics.

## Workflow Diagram
Add an architecture diagram or process flow chart here for better visualization.

## How to Run the Project

1. *Upload Raw Data*  
   Add .csv and .json files to the youtube-trend-analysis-raw-useast1-dev S3 bucket.

2. *Create Lambda function and ETL jobs using the files in the repository*
   
    - lambda-function.py - create Lambda funtion using the using and add a trigger with S3 raw bucket as source.
    - youtube-trend-analysis-cleaned-csv-to-parquet.py - Create a ETL job using the PySpark to convert csv to parquet format
    - youtube-trend-analysis-parquet-analytics-version.py - Create an ETL job using the script to join raw and reference data to parquet format

3. *Trigger Data Processing*  
   - Lambda will process .json files and store them in the cleaned bucket.
   - Run the Glue ETL jobs to transform .csv files and join datasets.

4. *Query Data*  
   Use Athena to query the db_youtube_analytics database for insights.

6. *Visualize Trends*  
   Create dashboards in QuickSight connected to the db_youtube_analytics database to analyze trends.

## Key Features

- *Serverless Architecture*: Fully scalable with minimal operational overhead.
- *Real-time Processing*: Automated triggers for seamless data flow.
- *Data Transformation*: Efficient conversion and joining of large datasets.
- *Interactive Dashboards*: QuickSight for visualizing YouTube trends.

## Future Enhancements

- Add real-time streaming using AWS Kinesis for live trend analysis.
- Incorporate machine learning models to predict future trends.
- Automate QuickSight dashboard updates with dynamic datasets.

## Contributors

- Srikar Reddy Nelavetla
- Kundan Sai Chowdary Sannapaneni

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
