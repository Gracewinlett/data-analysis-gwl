# 📊 Exploratory Data Analysis (EDA)

---

## 📄 Project Description
This project focuses on performing exploratory data summarization for the Academics Department's Attendance Procedure. The aim is to extract structured academic datasets stored in the S3 bucket which are already cleaned and profiled using DataBrew and then load them into AWS Glue Crawlers to create a Data Catalog. From there, AWS Glue ETL jobs are used to generate summarized datasets for reporting and performance insights.

---

## 🏷️ Project Title
Exploratory Data Analysis using Summarization on Attendance Datasets for Academic Department

---

## 🚀 Objective
- Retrieve cleaned data from tranformed S3 bucket.
- Use AWS Glue crawler to catalog data.
- Summarize the data using AWS Glue ETL jobs.
- Store results in S3 Curated Bucket and catalog in AWS Glue Data Catalog.

---
## 📊 Dataset
The Attendance datasets consist of below tables
- Attendance List
- Student List
- Instructor List
- Program List
- Course List

---

## ⚙️ Methodology

Below figure shows that overall architecture of the Data Analysis Platform using AWS for Academics Office.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/37eca5856c64a2d47ca78a9993c5fe89ac64785b/images/1.1.png)

### 1. Data Cataloging
We assume that raw Datasets in S3 bucket (Academics-raw) are already profiled, cleaned and stored in transformed buckets (Academics-trf). Those datasets will be registered into the Glue Data Catalog using AWS Glue Crawler. Create academics-crw and do data catalog. 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.2.png)


After running the crawler then catalog tables will be shown under Data Catalog.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.3.png)
 
Verified the cataloged data in AWS Glue and checked tables’ schemas.
### 2. Data Summarization
Glue ETL jobs are then used to transform and summarize the attendance data. Summarized output is stored in the curated S3 bucket, partitioned by report date, and can be used for descriptive and visual analysis.

Created an AWS Glue ETL job “Attendances-list-Summarization" as below.
-	Extract “Attendance List” data from data catalog
-	Transform
  -	Group by
    - attendancedate
    - courseid
    - instructorid
    - attendedstatus
  -	Aggregate
    - studentid countDistinct
-	Transform - add Report_Date to summary
-	Transform - convert to Report_Date_LTZ
-	Transform - prepare for load, remove Report_Date column
-	Data target - load to system (parquet format, snappy compression)
-	Transform - convert to one file
-	Data target - load to user (csv)
  
 ![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.4.png)

Created an AWS Glue ETL job “Students-list-Summarization" as below.
-	Extract “Student List” data from data catalog
-	Transform
  -	Group by
    -	programid
    -	status
  -	Aggregate
    -	studentid count
-	Transform - add Report_Date to summary
-	Transform - convert to Report_Date_LTZ
-	Transform - prepare for load, remove Report_Date column
-	Data target - load to system (parquet format, snappy compression)
-	Transform - convert to one file
-	Data target - load to user (csv)
  
 ![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.5.png)
 
Created an AWS Glue ETL job “Instructors-list-Summarization" as below.
-	Extract “Instructor List” data from data catalog
-	Transform
  -	Group by
    -	department
    -	status
  -	Aggregate
    -	instructorname count
-	Transform - add Report_Date to summary
-	Transform - convert to Report_Date_LTZ
-	Transform - prepare for load, remove Report_Date column
-	Data target - load to system (parquet format, snappy compression)
-	Transform - convert to one file
-	Data target - load to user (csv)

 ![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.6.png)
 
Created an AWS Glue ETL job “Programs-list-Summarization" as below.
-	Extract “Program List” data from data catalog
-	Transform
  -	Group by
    -	Department
    -	programdirector
    -	programlength
  -	Aggregate
    -	programname countDistinct
    -	creditsrequired avg
    -	programlength avg
-	Transform - add Report_Date to summary
-	Transform - convert to Report_Date_LTZ
-	Transform - prepare for load, remove Report_Date column
-	Data target - load to system (parquet format, snappy compression)
-	Transform - convert to one file
-	Data target - load to user (csv)

 ![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.7.png)
 
Created an AWS Glue ETL job “Courses-list-Summarization" as below.
-	Extract “Course List” data from data catalog
-	Transform
  -	Group by
    -	coursename
    -	semester
  -	Aggregate
    -	courseid count
-	Transform - add Report_Date to summary
-	Transform - convert to Report_Date_LTZ
-	Transform - prepare for load, remove Report_Date column
-	Data target - load to system (parquet format, snappy compression)
-	Transform - convert to one file
-	Data target - load to user (csv)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.8.png)
 
### 3. Data Storage
Data generated from ETL summarization jobs will be stored in curated bucket (Academics-cur). System friendly files (parquet format with snappy compression) will be stored under the system folders of respective data list folders as shown in the screenshots below.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.9.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.10.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.11.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.12.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.13.png)
  
 
User friendly files (csv format) will be stored under user folders of respective data list folders as shown in the screenshots below.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.14.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.15.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.16.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.17.png)
 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.18.png)
 
Summarization Output
| Dataset        | Summarization Job         | Output Location                                  |
|----------------|----------------------------|--------------------------------------------------|
| Attendance     | `Attendances-list-Summarization` | `S3://academics-cur-twl/attendances-list/metrics/` |
| Student        | `Students-list-Summarization`     | `S3://academics-cur-twl/students-list/metrics/`    |
| Instructor     | `Instructors-list-Summarization`  | `S3://academics-cur-twl/instructors-list/metrics/` |
| Program        | `Programs-list-Summarization`     | `S3://academics-cur-twl/programs-list/metrics/`    |
| Course         | `Courses-list-Summarization`      | `S3://academics-cur-twl/courses-list/metrics/`     |

### 4. Monitoring and log
The ETL jobs runs history information will be available under job run monitoring as shown in below.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/62b912a5482b826de8d898fe99ac40625049a5d1/images/1.19.png)

---
## 🧰 Tools and Technologies
- Amazon S3 – transformed and curated data storage
- AWS Glue Crawlers – Cataloging structured datasets
- AWS Glue ETL – Data transformation and summarization
- AWS Glue Data Catalog – Central metadata repository
- Amazon Athena – Future querying and analysis (not in scope here)

---

## 📦 Deliverables
- Datasets in the AWS Glue Data Catalog
- Summarized attendance datasets generated through ETL jobs
- Output files stored in system and user folders under curated S3 bucket

---
# 🎯 Descriptive Analysis

---

## 📄 Project Description
This project performs a descriptive analysis on the City of Vancouver’s business license dataset, specifically filtered for the business type “Caterer”. The analysis aims to derive insights on how business license statuses, issuance patterns, and employee distributions vary across locations and time periods. Data profiling, cleaning, cataloging, and summarization were carried out using AWS tools to prepare the dataset for meaningful interpretation.

---

## 🏷️ Project Title
Business License Descriptive Analysis – Business Type "Caterers"

---

## 🚀 Objective
To explore and analyze the distribution, status, and characteristics of business licenses categorized under “Caterer” in the City of Vancouver. The goal is to understand trends over time and across locations.

---
## 📊 Dataset
- **Source**: [City of Vancouver Open Data Portal](https://opendata.vancouver.ca/explore/dataset/business-licences/information/)
- **Theme**: Business and Economy
- **Filter Applied**: BusinessType = Caterer
- **Total Records**: 485
- **Extracted on**: February 24, 2025
- **Columns**: FolderYear, LicenceRSN, LicenceNumber, Status, IssuedDate, ExpiredDate, City, LocalArea, NumberOfEmployees, FeePaid, etc.

---

## ⚙️ Methodology

Below figure is the overall architecture of the Data Analysis Platform using AWS for this descriptive analysis.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/bdfd246e94bb2d4bf8d09dc8f0606973641f5d8a/images/2.1.png)


### 1. Data Ingestion
Downloaded dataset from City of Vancouver Open Data Portal filtered for business type = "Caterer" (Figure 2.1).

Uploaded the raw dataset (CSV) into the Amazon S3 raw bucket at:
/businesseconomy/business-license/year=2025/quarter=01/month=02/day=24/server=BGVS-Twl
(Figure 2.2).

### 2. Data Profiling
Used AWS Glue DataBrew to create a profile job for raw dataset to assess data completeness and identify anomalies (Figure 2.4).

Job results and profiling logs were stored back in S3 (Figures 2.5 and 2.6).

### 3. Data Cleaning
Created a cleaning recipe in AWS Glue DataBrew (Figure 2.7).

Tasks included:

Trimming white spaces from multiple fields.

Replacing "British Columbia" with "BC".

Filling missing dates with “1111-11-11”.

Imputing missing FeePaid with the most frequent value.

Assigning “Hidden” to missing LocalArea fields.

Outputs stored in both system and user folders in S3 (Figures 2.8, 2.9, 2.10).

### 4. Data Cataloging
Configured AWS Glue Crawler to crawl transformed data from S3 (Figure 2.11).

Successfully created and verified catalog tables in Glue Data Catalog (Figure 2.12).

### 5. Data Summarization
Used AWS Glue Visual ETL to create “Business-License-Summarization” job (Figure 2.13).

- Extracted from data catalog.
- Dropped unused columns.
- Filtered relevant statuses (Issued, Inactive, etc.).
- Grouped by status, city, localarea, and issued year.
- Aggregated count of distinct License RSN, average employees, and average fees.
- Appended Report_Date field and converted to local time zone.
- Loaded to both system and user folders in the curated S3 bucket (Figures 2.17, 2.18).
- Created a summarized output schema and cataloged it in AWS Glue (Figures 2.15, 2.16).





![image]()


---
## 🛠️ Tools and Technologies
- Amazon S3 – data storage
- AWS Glue DataBrew - Profiling, Cleaning
- AWS Glue Crawlers – Cataloging structured datasets
- AWS Glue ETL – Data transformation and summarization
- AWS Glue Data Catalog – Central metadata repository
- Amazon Athena – Future querying and analysis (not in scope here)

---

## 📦 Deliverables
- Cleaned and structured dataset in S3
- Data profiling and transformation recipe (Glue DataBrew)
- Summarized license metrics stored in system/user folders
- AWS Glue ETL job for data summarization
- Catalog tables 

---
# 🔄 Data Wrangling

---

## 📄 Project Description

---

## 🏷️ Project Title


---

## 🎯 Objective

---
## 🧠 Background


---
## 🗂️ Dataset


---

## ⚙️ Methodology



### 1. Data Cataloging

![image]()


---
## 🧰 Tools and Technologies
- Amazon S3 – transformed and curated data storage
- AWS Glue Crawlers – Cataloging structured datasets
- AWS Glue ETL – Data transformation and summarization
- AWS Glue Data Catalog – Central metadata repository
- Amazon Athena – Future querying and analysis (not in scope here)

---

## 📦 Deliverables
- tt

---
## 📆 Timeline
- tt

---
# 📊 Data Quality Control
 

---

## 📄 Project Description

---

## 🏷️ Project Title


---

## 🚀 Objective


---
## 📊 Background

---
## 📌 Scope

---

## ⚙️ Methodology



### 1. Data Cataloging

![image]()


---
## 🧰 Tools and Technologies
- Amazon S3 – transformed and curated data storage
- AWS Glue Crawlers – Cataloging structured datasets
- AWS Glue ETL – Data transformation and summarization
- AWS Glue Data Catalog – Central metadata repository
- Amazon Athena – Future querying and analysis (not in scope here)

---

## 📦 Deliverables
- tt

---
## 📆 Timeline
- tt

---
# 📊 Course Completion Badge

 


---

## 📃 References
- AWS Academy. (2022). *AWS Academy Cloud Foundations*. Retrieved from https://awsacademy.instructure.com/courses/106917/modules

---

## 👤 Author
**Grace Winlett**  
GitHub: [Gracewinlett](https://github.com/Gracewinlett)

---


