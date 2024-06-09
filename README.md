# Unlocking-Real-Time-Data-Insights-Seamless-Integration-between-Snowflake-and-Power-BI

# Business Scenario: 
Last week, I delved into data analysis and visualization for Call Centre Data using Microsoft Power BI. With approximately 15K data entries in a CSV file, I meticulously transformed the data to identify inconsistencies and diminish data issues. Subsequently, I crafted insightful dashboards by unraveling trends and patterns.
Upon successfully creating the initial dashboard and report, I encountered a new challenge of importing data from multiple CSV files into the existing report efficiently. By using Power BI feature of appending queries, every time I need to manually add, transform and append dataset. Seeking a more streamlined approach, I opted for an alternate solution that allows data retrieval from multiple files without the need of manual intervention for creation of dashboards and reports in Power BI. 💡

# Implementation Process

# Step 1: Real-Time Data Loading in Snowflake

I successfully utilized Snowflake's real-time data loading capabilities to seamlessly ingest data from a Google storage bucket into Snowflake tables using Snowpipe. 

The integration flow is illustrated in the diagram below:

![image](https://github.com/Donaldgonsalves398/Unlocking-Real-Time-Data-Insights-Seamless-Integration-between-Snowflake-and-Power-BI/assets/160304092/8bf40899-a37d-4362-84ae-601ca3a2c69a)

-> User uploads files to the Google storage bucket.
-> Cloud storage notification service triggers an event [Pub / Sub notification] to the Snowflake Snowpipe object and informs Snowpipe about newly added files ready to be loaded into the Snowflake table.
-> Snowpipe extracts the data file from the Google storage bucket and loads it into the table.

Note: Snowpipe efficiently prevents the reloading of the same files by using file-loading metadata associated with each pipe object. Hence, if a duplicate file appears in the Google storage bucket, Snowpipe will not reload it.

# Steps
GCP Storage Bucket: Created an Storage bucket on GCP using an IAM user with appropriate premission. Snowflake requires the following permissions on an storage bucket and folder to be able to access files in the folder - 
storage.buckets.get
storage.objects.create
storage.objects.delete
storage.objects.get
storage.objects.list
Also, assign snowflakes service account name to storage bucket.

GCP Pub/Sub Configuration: Created Pub/Sub Topic and Pub/Sub Subscription and assign snowfalkes service account created during Notification Integration Stage.

Snowflake Table: Created a Table in Snowflake Datawarehouse as per datatypes identified in Data Profiling steps. Data will be loaded in this table.

External Stage: Created an external stage that references the GCP Storage bucket where the data file is stored. External Stage is used to load data which is stored externally (e.g. GCP Storage Bucket, Amazon S3, Azure blob storage) into tables in a Snowflake database.

Snowpipe: It is a continuous data ingestion service that automatically loads new data as it arrives in a specified location. It fetches the data files from the external stage and temporarily queue them before loading them into the target table. Provided file format as well while creating the snowpipe.

More details can be found in the SQL file attached.

Log of data being loaded into snowflake using snowpipe:-

# Step 2: Data Transformation & Visualization in Microsoft Power BI

-> Loaded the data into Power BI by creating a connection with Snowflake Data warehouse.
-> Transformed the data in Power BI and imported data by selecting the option as a DirectQuery, then visualized the data by identifying insights.
-> When a new file is uploaded into the Google storage bucket, we need to refreshed Power BI reports and reports get updated.

# References: 
Configuring an integration for Google Cloud Storage: https://docs.snowflake.com/en/user-guide/data-load-gcs-config

Automating Snowpipe for Google Cloud Storage: https://docs.snowflake.com/en/user-guide/data-load-snowpipe-auto-gcs
