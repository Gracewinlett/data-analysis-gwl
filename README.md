# 📊 Exploratory Data Analysis (EDA)

---

## 📅 Project Description
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



---


---



---

## 📃 References
- AWS Academy. (2022). *AWS Academy Cloud Foundations*. Retrieved from https://awsacademy.instructure.com/courses/106917/modules

---

## 👤 Author
**Grace Winlett**  
GitHub: [Gracewinlett](https://github.com/Gracewinlett)

---


