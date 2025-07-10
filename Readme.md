# 🌾 Agriculture Data Analysis using AWS S3, Snowflake & Power BI

This end-to-end project demonstrates how to upload and analyze agricultural data using AWS S3 for storage, Snowflake for cloud warehousing and transformation, and Power BI for interactive visualization. It covers rainfall, temperature, humidity, and yield patterns across time, locations, and crops.

---
## ✅ Project Workflow Summary

### 🔧 Step-by-Step Implementation

1. **Create S3 Bucket in AWS**
   - Bucket Name: `snowflake.powerbiproject` (use lowercase)
   - Upload your data file `data_season.csv` to this bucket.

2. **Create IAM Role**
   - Role Name: `snowflake.powerbiproject.role`
   - Attach trust policy to allow Snowflake to assume this role.

3. **Snowflake Storage Integration**
   - Create a Snowflake storage integration object:
     ```sql
     CREATE OR REPLACE STORAGE INTEGRATION Powerbiproject_Integration
       TYPE = EXTERNAL_STAGE
       STORAGE_PROVIDER = 'S3'
       ENABLED = TRUE
       STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::585008085138:role/snowflake.powerbiproject.role'
       STORAGE_ALLOWED_LOCATIONS = ('s3://snowflake.powerbiproject/');
     ```

4. **Load Data into Snowflake**
   ```sql
   CREATE DATABASE powerbi;
   CREATE SCHEMA pbi_data;

   CREATE TABLE pbi_dataset (
     Year INT,
     Location STRING,
     Area INT,
     Rainfall FLOAT,
     Temperature FLOAT,
     Soil_Type STRING,
     Irrigation STRING,
     Yields INT,
     Humidity FLOAT,
     Crops STRING,
     Price INT,
     Season STRING
   );

   CREATE STAGE powerbi.pbi_data.pbi_stage1
     URL = 's3://snowflake.powerbiproject'
     STORAGE_INTEGRATION = Powerbiproject_Integration;

   COPY INTO pbi_dataset
   FROM @pbi_stage1
   FILE_FORMAT = (TYPE = CSV FIELD_DELIMITER = ',' SKIP_HEADER = 1)
   ON_ERROR = 'continue';
## 🔄 Data Transformation in Snowflake

### 🧪 Create Working Table
CREATE TABLE agriculture AS SELECT * FROM pbi_dataset;


**🔧 Perform Transformations**
 ```sql
 Update Columns
 UPDATE agriculture SET Rainfall = 1.1 * Rainfall;
 UPDATE agriculture SET Area = 0.9 * Area;

 Add Year Groups
 ALTER TABLE agriculture ADD Year_Group STRING;

 UPDATE agriculture SET Year_Group = 'Y1' WHERE Year BETWEEN 2004 AND 2009;

 UPDATE agriculture SET Year_Group = 'Y2' WHERE Year BETWEEN 2010 AND 2015;

 UPDATE agriculture SET Year_Group = 'Y3' WHERE Year BETWEEN 2016 AND 2019;

 Add Rainfall Group
 ALTER TABLE agriculture ADD Raingroup_Column STRING;

 UPDATE agriculture SET Raingroup_Column = 'Low'    WHERE Rainfall BETWEEN 255 AND 1200;

 UPDATE agriculture SET Raingroup_Column = 'Medium' WHERE Rainfall BETWEEN 1201 AND 2800;

 UPDATE agriculture SET Raingroup_Column = 'High'   WHERE Rainfall BETWEEN 2801 AND 4500;
```


## 📊 Power BI Dashboards

All visuals are created using the transformed data from the `agriculture` table:

1. 🌧 **Rainfall Analysis**
2. 🌡 **Temperature Analysis**
3. 💧 **Humidity Analysis**
4. 🌾 **Yield Analysis**

---

## 💡 Key Learnings

- 🚀 **End-to-end pipeline** from S3 to Power BI  
- 🔐 **Snowflake role-based access** and external stage setup  
- 🛠️ **SQL-based feature engineering** using `ALTER`, `UPDATE`, and logic classification  
- 📈 **Insights on Rainfall, Temperature, Humidity, and Yield**

---

## 🧰 Tools Used

| 🧪 Tool       | 🔍 Purpose                            |
|--------------|----------------------------------------|
| AWS S3        | Cloud storage for raw CSV files        |
| Snowflake     | Scalable cloud data warehousing        |
| Power BI      | Dashboarding and data visualization    |
| SQL           | Data transformation and preparation    |

---

## 📂 Project Files

- 📁 `data_season.csv` – Raw dataset  
- 📊 `Agriculture_Analysis_PowerBi.pbix` – Final Power BI Report  
- 🖼️ Dashboard Screenshots:
  - `Rainfall_analysis_ss.jpg`
  - `Temperature_analysis_SS.jpg`
  - `Humidity_analysis_SS.jpg`
  - `Yield_analysis_ss.jpg`

---

## 🙋‍♀️ Author

Built with ❤️ by **Ashwini Koppula**  
[GitHub: AshwiniR2025](https://github.com/AshwiniR2025)



