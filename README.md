# AgriFlow ETL Pipeline

## Overview
AgriFlow is a fully Dockerized ETL pipeline built to streamline the extraction, transformation, and loading of USDA data. By leveraging MongoDB, PySpark, Pandas, and PostgreSQL, the pipeline ensures an efficient and reliable flow of data from the USDA API into a centralized database, ready for analysis. It relies on the organized structure of the `etl_components` and `scripts` directories to tie all stages of the process together seamlessly.

https://hub.docker.com/repository/docker/fmahmud922/agriflow/tags/lts/sha256:a826c7b84669d6f2113f2192db20f4426638f67b9a3d5e3f0a70c963e003f78c

## Workflow
1. **MongoDB Initialization**: The process begins with setting up a MongoDB instance, hosted in a personal AWS cluster.
2. **Data Transformation**: Raw data from MongoDB collections is imported into PySpark for cleaning, schema definition, and NULL value handling. The data is then converted into Pandas dataframes for further manipulation.
3. **Dataframe Union**: All Pandas dataframes are combined into a single, unified dataframe to ensure consistency and simplify loading.
4. **Database Loading**: The unified dataframe is prepared for PostgreSQL by defining the schema, clearing any existing landing tables, and using the `execute_values` function for efficient bulk insertion.

---

## Instructions  

**1.** Ensure all environment variables are correctly configured in a `.env` file within the project root directory before running the pipeline.    

```env
# USDA API Key  
usda_key=XXXXXXX  

# MongoDB Credentials  
mongo_username=XXXXXXX  
mongo_password=XXXXXXX  
mongo_default_clusterName = XXXXXXXX

# MongoDB Connection URI  
mongo_client=XXXXXXXX  

# Mongo Cluster Name
mongo_default_clusterName=XXXXXXXX

# PostgreSQL (pgAdmin4) Login Credentials  
pgadmin_user=XXXXXXX  
pgadmin_password=XXXXXXX

#Must define landing db, table, and schema names to be dumped in PostgreSQL.
landing_db=XXXXXX
landing_table=XXXXXXX (with format [table_schema].[table_name])
```

**2.** Have Docker Engine installed on your local machine with this link, and run the following commands in the command line. \
https://docs.docker.com/engine/install/

```
#Run this code first
docker pull fmahmud922/agriflow:lts 

#Run this afterwards.
docker run --env-file .env fmahmud922/agriflow:lts
```


## Important Notes
- **Silent Issue in `get_counts`**: Occasionally, the `get_counts` function, responsible for fetching data from the USDA API, may fail silently and cause the process to stall indefinitely.  
  - **Temporary Fix**: To avoid this, pull from fmahmud/agriflow:dev.

---

## Future Updates
Looking ahead, the following enhancements are planned for AgriFlow:
- **Apache Airflow**: Integrating Airflow to automate and schedule ETL workflows.
- **DBT Integration**: Incorporating DBT to reinforce data consistency and maintain high data quality standards.
