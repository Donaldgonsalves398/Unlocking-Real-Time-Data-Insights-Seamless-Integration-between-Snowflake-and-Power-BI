# Unlocking-Real-Time-Data-Insights-Seamless-Integration-between-Snowflake-and-Power-BI

# Business Scenario: Last week, I delved into data analysis and visualization for Call Centre Data using Microsoft Power BI. With approximately 15K data entries in a CSV file, I meticulously transformed the data to identify inconsistencies and diminish data issues. Subsequently, I crafted insightful dashboards by unraveling trends and patterns.
Upon successfully creating the initial dashboard and report, I encountered a new challenge of importing data from multiple CSV files into the existing report efficiently. By using Power BI feature of appending queries, every time I need to manually add, transform and append dataset. Seeking a more streamlined approach, I opted for an alternate solution that allows data retrieval from multiple files without the need of manual intervention for creation of dashboards and reports in Power BI. 💡

Step 1: Real-Time Data Loading in Snowflake

I successfully utilized Snowflake's real-time data loading capabilities to seamlessly ingest data from a Google storage bucket into Snowflake tables using Snowpipe. 

The integration flow is illustrated in the diagram below:

-> User uploads files to the Google storage bucket.
-> Cloud storage notification service triggers an event [Pub / Sub notification] to the Snowflake Snowpipe object and informs Snowpipe about newly added files ready to be loaded into the Snowflake table.
-> Snowpipe extracts the data file from the Google storage bucket and loads it into the table.

Note: Snowpipe efficiently prevents the reloading of the same files by using file-loading metadata associated with each pipe object. Hence, if a duplicate file appears in the Google storage bucket, Snowpipe will not reload it.

Step 2: Data Transformation & Visualization in Microsoft Power BI

-> Loaded the data into Power BI by creating a connection with Snowflake Data warehouse.
-> Transformed the data in Power BI and imported data by selecting the option as a DirectQuery, then visualized the data by identifying insights.
-> When a new file is uploaded into the Google storage bucket, we need to refreshed Power BI reports and reports get updated.