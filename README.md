# Power BI Integration with AWS S3 and Snowflake

## Project Overview

This project demonstrates an end-to-end data pipeline where a CSV file is uploaded to AWS S3, ingested into Snowflake, and then analyzed using Power BI. The integration enables efficient data processing, storage, and visualization for data-driven decision-making.

## 1. Data Ingestion from AWS S3 to Snowflake

### Step 1: Creating an AWS S3 Bucket
  - Log in to AWS Management Console.
  - Navigate to S3 and click Create bucket.
  - Provide a unique Bucket Name and choose a region.
  - Configure settings and adjust permissions if required.
  - Click Create bucket.

### Step 2: Uploading CSV File to S3
  - Open the created S3 bucket.
  - Click Upload and select the CSV file from your local system.
  - Configure permissions as needed and click Upload.

### Step 3: Creating a Table in Snowflake
Run the following SQL command in Snowflake Worksheet to create a table with appropriate schema:

create table PBI_Dataset (
Year int,	Location string,	Area	int,
Rainfall	float, Temperature	float, Soil_type string,
Irrigation	string, yeilds	int,Humidity	float,
Crops	string,price	int,Season string
);

### Step 4: Creating an External Stage for S3 in Snowflake

create stage PowerBI.PBI_Data.pbi_stage
url = 's3://powerbi.project.snowflake'
storage_integration = PBI_Integration

### Step 5: Loading CSV Data into Snowflake

copy into PBI_Dataset 
from @pbi_stage
file_format = (type=csv field_delimiter=',' skip_header=1 )
on_error = 'continue'

## 2. Connecting Snowflake to Power BI
### Step 6: Configuring the Snowflake Connector in Power BI
  - Open Power BI Desktop.
  - Click Get Data → Select Snowflake.
  - Enter your Snowflake Server Name and Warehouse details.
  - Choose DirectQuery or Import Mode.
  - Select the Rainfall_Analysis table and click Load.

## 3. Power BI Data Modeling & Measures
## DAX Measures Created
### 1. Average Rainfall
Avg Rainfall = AVERAGE('Rainfall Analysis'[RAINFALL])

### 2. Total Revenue
Total Revenue = SUMX('Rainfall Analysis','Rainfall Analysis'[YEILDS]*'Rainfall Analysis'[PRICE])

### 3. Total Yield
Total Yield = SUM('Rainfall Analysis'[YEILDS])


# Power BI Service Dashboard
<img width="957" alt="image" src="https://github.com/user-attachments/assets/40febf6b-15cd-4ae1-a210-bbddf101ca46" />

# Power BI Desktop
<img width="959" alt="image" src="https://github.com/user-attachments/assets/3f05b593-0f61-4e5c-a463-84a80926882a" />




