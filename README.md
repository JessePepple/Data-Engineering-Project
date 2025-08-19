# Data-Engineering-Project
This project implemented Unity Catalog in Databricks to provide secure, centralized data governance, while leveraging Delta Lake and Delta Tables for scalable and efficient data storage. The data pipeline followed the Medallion Architecture (Bronze → Silver → Gold), ensuring data quality, reliability, and usability at every stage. The solution was built using Azure Data Factory, Azure SQL Database, Azure Data Lake Storage, and Azure Databricks, with Python and SQL as the primary languages for data transformation and scripting.

Azure Services:
Azure Databricks
Azure Data Factory (ADF)
Azure Data Lake Storage (ADLS Gen2)
Azure SQL Database
Azure Synapse Analytics

Databricks Components:

Delta Lake & Delta Tables
Unity Catalog (governance)
Databricks SQL Warehouse

Languages & Tools:

Python (PySpark / notebooks)
SQL

For detailed Storytelling regarding this project please visit this link:
https://www.jesseportfolio.co.uk/post/cars-data-engineering-project


## PROJECT OVERVIEW
<img width="1323" height="736" alt="Project1 Overview" src="https://github.com/user-attachments/assets/d7ba01c6-7acc-4bca-bee6-36ab96d920a4" />

## CREATING PIPELINE FOR THE DATA SOURCE FROM GITHUB AND THEN STORING IT IN SQL DB

After provisioning and organizing my resources within a dedicated Azure Resource Group, I began the data ingestion process using Azure Data Factory. The raw data was sourced directly from GitHub and ingested into Azure SQL Database, establishing a reliable foundation for subsequent transformation and processing.

<img width="1434" height="737" alt="Screenshot 2025-09-01 at 03 24 09" src="https://github.com/user-attachments/assets/f73d5133-5622-477e-b65b-26e3eda43441" />

<img width="1434" height="737" alt="Screenshot 2025-09-01 at 03 29 19" src="https://github.com/user-attachments/assets/31270239-b012-4c9f-91cd-33dbe77e0e75" />

Also in preparation for our initial and incremental loading I tested out a stored procedure setting down the requirements for our incremental pipeline

<img width="1434" height="737" alt="Screenshot 2025-09-01 at 04 13 21" src="https://github.com/user-attachments/assets/7d16fc75-6c2b-4221-b154-eef916cc919d" />

## Initial and Incremental Loading

The next stage involved building a pipeline to move data from Azure SQL Database into Azure Data Lake Storage. A key focus at this stage was designing both initial loading and incremental loading processes to ensure efficient and consistent data updates. For incremental loading, the current load was determined using the query: SELECT MAX(Date_ID) AS max_date 
FROM source_cars_data; Whilst the last_laod was SELECT * FROM water_table;

<img width="1434" height="737" alt="Screenshot 2025-09-01 at 06 05 50" src="https://github.com/user-attachments/assets/662dd125-5508-4914-a286-ffcf023a6c50" />

## Phase2(Transformation)

The project also utilized Unity Catalog to provide secure and centralized data governance, followed by the setup of the Databricks environment for the core transformation tasks. As demonstrated in the Silver Notebook, the transformations required at this stage were relatively minimal. The most notable adjustment was splitting the Model_Id field into a new Model Category, which laid the groundwork for handling Slowly Changing Dimensions (SCDs Type 1) and preparing the data for further refinement.

<img width="1434" height="737" alt="Screenshot 2025-09-01 at 07 56 14" src="https://github.com/user-attachments/assets/b4347fef-eec4-41f0-9fd6-a970d0ecb757" />
<img width="1434" height="737" alt="Screenshot 2025-09-02 at 01 52 38" src="https://github.com/user-attachments/assets/17a40028-3988-47f5-bbb3-99876f5a7dc3" />
<img width="1434" height="737" alt="Screenshot 2025-09-01 at 11 12 19" src="https://github.com/user-attachments/assets/33703ee4-b091-410c-a54a-d66ef245c4ee" />

## Phase3 (Serving)
In Phase 3, the primary focus was on implementing the Slowly Changing Dimensions (SCD) process, which involved cleaning the data and creating the necessary keys for the final tables. This step included building the dimension model and generating surrogate keys for entities such as DealerName/DealerID, ModelID/ModelCategory, Date_ID, and BranchID/BranchName. These dimensions were then joined together in the Fact Sales table to finalize the Star Schema, ensuring the data was optimized for analytics and reporting. Once the schema was complete, I planned to schedule a pipeline for both the initial load (stored in the Databricks warehouse) and the incremental load, enabling the pipeline to refresh efficiently as new data arrives.

<img width="1434" height="737" alt="Screenshot 2025-09-03 at 10 50 21" src="https://github.com/user-attachments/assets/53eadc36-6618-4ff9-98cd-60c613374b09" />

<img width="1434" height="737" alt="Screenshot 2025-09-03 at 06 12 41" src="https://github.com/user-attachments/assets/bce53109-3379-48cc-93c4-fe3d304016d5" />


## The Final Pipeline

The Initial Load was successful
<img width="1434" height="737" alt="Screenshot 2025-09-03 at 07 07 33" src="https://github.com/user-attachments/assets/acf50df7-6930-473b-9507-bc4894d058cf" />
To further validate the pipeline, I introduced new changes to the source data and re-ran the process. The updates were handled successfully, confirming that the Slowly Changing Dimensions (SCD Type 1) logic worked as intended. This final step demonstrated the robustness of the design and fully concluded the project with a reliable and scalable data pipeline.

<img width="1434" height="737" alt="Screenshot 2025-09-03 at 11 02 13" src="https://github.com/user-attachments/assets/7d1b2c6c-dd9d-479b-ab11-c3ba484896ee" />

To validate the final output, I tested the data by running SQL queries in the Databricks SQL Warehouse, simulating the type of exploration a data analyst would perform. This step confirmed that the data was well-modeled, accurate, and ready for analysis, thereby concluding the project and demonstrating that the pipeline successfully delivered data that is both prepared and business-ready.

## Lesson Learnt

Working through this project provided valuable insights into the end-to-end data engineering lifecycle. A few key takeaways include:

Importance of Governance – Leveraging Unity Catalog reinforced how crucial centralized governance and security are when working with multiple data sources and teams.

Incremental vs. Initial Loads – Designing for both scenarios showed the importance of efficiency and scalability when building pipelines that must refresh regularly.

Transformation Complexity – Even “light” transformations in the Silver layer can have downstream impacts, especially when setting up keys and preparing for Slowly Changing Dimensions (SCDs).

Data Modeling Best Practices – Building the Star Schema emphasized the importance of carefully creating dimension and fact tables, along with surrogate keys, to ensure the model supports analytical queries effectively.

Overall, this project helped strengthen my ability to design and implement scalable pipelines while balancing governance, performance, and usability.














