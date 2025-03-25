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
## 🛠️ Tools and Technologies
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
This project performs a descriptive analysis on the City of Vancouver’s business license dataset, specifically filtered for the business type “Caterer”. The analysis aims to derive insights on how business license statuses, issuance patterns, and employee distributions vary across locations and time periods. Data ingestion, profiling, cleaning, cataloging, and summarization were carried out using AWS tools to prepare the dataset for meaningful interpretation.

---

## 🏷️ Project Title
Business License Descriptive Analysis – Business Type "Caterers"

---

## 🚀 Objective
To explore and analyze the distribution, status, and characteristics of business licenses categorized under “Caterer” in the City of Vancouver. The goal is to summarize and prepare data to analysis trends over time and across locations.

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
Downloaded dataset from City of Vancouver Open Data Portal filtered for business type = "Caterer".

Uploaded the raw dataset (CSV) into the Amazon S3 raw bucket at:
/businesseconomy/business-license/year=2025/quarter=01/month=02/day=24/server=BGVS-Twl.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.2.png)

Another S3 bucket created to store data for data transformation. 
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.3.png)

### 2. Data Profiling
Used AWS Glue DataBrew to create a profile job for raw dataset to assess data completeness and identify anomalies. Using that, upload dataset to transform S3 bucket and then create a data profiling job as shown in figure.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.4.png)

Job result has generated as shown in below and profiling logs were stored back in S3 transformed bucket.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.5.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.6.png)

### 3. Data Cleaning
Data Cleaning is needed to make sure the dataset is structured, accurate, and usable for analysis. Created a cleaning recipe in AWS Glue DataBrew.

Tasks included:
- Remove white spaces from BusinessName, LicenceNumber, BusinessTradeName, Status,	BusinessType, BusinessSubType,	Unit,	UnitType,	Street,	City,	Province,	Country,	PostalCode, LocalArea.
- Replace text British Columbia with BC in Province
- Fill missing values with "1111-11-11" in IssuedDate and ExpiredDate
- Fill missing values with most frequent value in FeePaid
- Fill missing values with "Hidden" in LocalArea
  
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.7.png)

The result of the receipe job will be shown as below.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.8.png)

Outputs stored in both system and user folders in S3 transformed bucket.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.9.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.10.png)

### 4. Data Cataloging
Configured AWS Glue Crawler to crawl transformed data from S3 to catalog the cleaned structured data.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.11.png)

Successfully created and verified catalog tables in Glue Data Catalog.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.12.png)

### 5. Data Summarization
Used AWS Glue Visual ETL to create “Business-License-Summarization” job to summarize and prepare the data for analysis.

- Extracted from data catalog.
- Transform scheme by dropping unnecessary columns (Licencerevisionnumber, Businesstradename,	Businesstype,	Businesssubtype,	Unit,	Unittype,	House,	Province,	Country,	Postalcode)
- Transform - Filter data by status which matched the status (Gone Out of Business, Inactive,	Issued, Pending)
- Transform date to year for license Issued date
- Transform data
  - Grouped by status, city, localarea, and issued year.
  - Aggregated count of distinct License RSN, average employees, and average fees.
- Transform - add Report_Date to summary
- Transform - convert to Report_Date_LTZ
- Transform - prepare for load, remove Report_Date column
- Data target - load to system (parquet format, snappy compression)
- Transform - convert to one file
- Data target - load to user (csv)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.13.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.14.png)

After successfully running ETL job, the metrics table for summarized result will be stored under Data catalog as shown in below figures. 

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.15.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.16.png)

The summarized data will be stored under the system and user folders of curator S3 bucket.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.17.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.18.png)

The detail monitoring information of the ETL job can be seen as shown below.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/7cd1456c72e35120c85129260c7ce86bc550815f/images/2.19.png)

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
- Catalog tables
- AWS Glue ETL job for data summarization
- Summarized license metrics stored in system/user folders

---
# 🧹 Data Wrangling

---

## 📄 Project Description
This project focuses on data wrangling for the Academics Department - Attendance Procedure. The goal is to clean, prepare, and structure raw academic data to ensure its quality and usability for downstream analytics such as descriptive analysis and reporting.

---

## 🏷️ Project Title
Academic Attendance Data Wrangling and Transformation

---

## 🎯 Objective
To perform comprehensive data profiling, cleaning, and transformation using AWS Glue DataBrew and AWS Glue ETL jobs to ensure consistency, completeness, and structure for analytical processes.

---
## 🧠 Background
The university’s Academics Department collects various datasets, including student, attendance, course details, instructor information, and program metadata. These datasets contain inconsistencies such as whitespace, missing values, and mixed formatting that need to be standardized for quality analytics.

---
## 🗂️ Dataset
- Student List
- Attendance List
- Instructor List
- Program List
- Course List

Each dataset contains 50 rows including string, date and integer types. They are uploaded to Amazon S3 raw bucket.

---

## ⚙️ Methodology

Below is the architecture diagram of Data Wrangling using AWS components.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/3db7f79e346189bb93a2f19167910d4e1a6002c4/images/3.1.png)

### 1. Data Ingestion
Datasets are uploaded to S3 raw bucket to start transformation.  

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/f51cfbe0b150ec1b884af94ef76ac68d5478a63d/images/3.1.1.png)

### 2. Data Profiling
Used AWS Glue DataBrew to analyze structure, missing values, and data types.  
Create profiling jobs for each raw dataset to get the profile.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.2.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.3.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.4.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.5.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.6.png)

All the profile jobs are successfully run and profiles are uploaded.
![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.6.1.png)

### 3. Recipe Creation & Data Cleaning
Created cleaning recipes for each dataset to:
   - Remove whitespace
   - Replace missing values
   - formate the date values to yyyy-mm-dd
   - Standardize columns formats
All receipes jobs are successfully run as shown in figure.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/9042c2bd270f775de2b154fe3e30fc37cca36eed/images/3.7.png)

### 4. Data Storage
Outputs stored in both system (parquet format with snappy compression) and user(csv format) folders in S3 transformed bucket.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/44eb6a5da5fb8fa6120b85b9483ac370bda19605/images/3.8.png)

---
## 🛠️ Tools and Technologies
- AWS S3 (raw and transformed)
- AWS Glue DataBrew (Data profiling and Cleaning)

---

## 📦 Deliverables
- Cleaned high quality datasets stored in transformed S3 bucket

---
## ⏳ Timeline
| Task | Duration |
|------|----------|
| Data Profiling | 30 mins |
| Cleaning Recipe Setup | 60 mins |
| Recipe Job Execution | 20 mins |

---
# 📊 Data Quality Control 

---

## 📄 Project Description
This project focuses on applying data quality checks to the business license dataset from the City of Vancouver using AWS Glue ETL. The objective is to ensure the data is clean, complete, and trustworthy before further analysis.

---

## 🏷️ Project Title
Data Quality Control for Business License Dataset for Business Type "Caterers" – City of Vancouver

---

## 🚀 Objective
To validate, clean, and assess the quality of the business license dataset by applying completeness, uniqueness, and freshness checks using AWS Glue ETL jobs and store the results into separate "Passed" and "Failed" folders for further action.

---
## 🧠 Background
Raw datasets often include inconsistent, missing, or outdated values, which can impact the accuracy of downstream analytics. To address this, the Data Analysis Platform includes a dedicated data quality control phase to validate key quality dimensions and support reprocessing or clean storage.

---
## 📌 Scope
- Validate data completeness for key fields such as FOLDERYEAR and LicenceRSN.
- Check the uniqueness of the primary key LicenceRSN.
- Ensure issued license dates are within the last 365 days for data freshness.
- Store passed and failed records into designated S3 folders for review and processing.

---

## ⚙️ Methodology

Below is the architecture diagram including monitoring using AWS components.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/2b5459252219af34896599c2900105975bac6753/images/4.1.png)

### 1. Build ETL Job for Data Quality Checks
An ETL job named bue-bul-QC-Twl was implemented using AWS Glue as below.

- Load Data: Load data from the S3 bucket.
- Custom Transform: Apply data quality rules.
  - Completeness: Ensure FOLDERYEAR and LicenceRSN > 0
  - Uniqueness: Ensure LicenceRSN is unique
  - Freshness: Ensure IssuedDate is less than 365 days from the current date
- Route rows: Use a conditional router to separate Passed and Failed data.
- Transform & Load: Store results into separate folders.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/68c7577c86a5413f10bd9ed136c663caf6a2863d/images/4.2.png)

### 2. Store Quality Results
Passed and Failed results in csv format are saved into corresponding folders in the S3 transformed bucket.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/68c7577c86a5413f10bd9ed136c663caf6a2863d/images/4.3.png)

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/68c7577c86a5413f10bd9ed136c663caf6a2863d/images/4.4.png)

### Monitoring
Create CloudTrail to keep track of all the activities so that events can continuously capture in the log and are easy to trace back in case anything happens.

![image](https://github.com/Gracewinlett/data-analysis-gwl/blob/68c7577c86a5413f10bd9ed136c663caf6a2863d/images/4.5.png)

---
## 🛠️ Tools and Technologies
- Amazon S3 – data storage
- AWS Glue – Visual ETL job creation and execution
- AWS CloudTrail – Logs activities
- Parquet + Snappy Compression – Efficient storage format for optimized I/O

---

## 📦 Deliverables
- AWS Glue ETL Job (bue-bul-QC-Twl)
- Data stored in Passed and Failed folders in S3 transformed bucket
- Activities Log from CloudTrail

---
## 📆 Timeline
| Task | Duration |
|------|----------|
| Build ETL job | 60 mins |
| ETL Job Execution | 10 mins |

---

# 📃 References
- AWS Academy. (2022). *AWS Academy Cloud Foundations*. Retrieved from https://awsacademy.instructure.com/courses/106917/modules
- 
---
# 🏆 Course Completion Badge

[View my AWS Academy Graduate - AWS Academy Cloud Foundations](https://www.credly.com/badges/dde90078-8fcc-4e2e-bb6e-981d6067608c/public_url)

---

# 👤 Author

**Grace Winlett**  

[LinkedIn](https://www.linkedin.com/in/grace-winlett-62262a55/)

[GitHub](https://github.com/Gracewinlett)

---


