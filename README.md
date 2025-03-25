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


## 📈 Summarization Output
| Dataset        | Summarization Job         | Output Location                                  |
|----------------|----------------------------|--------------------------------------------------|
| Attendance     | `Attendances-list-Summarization` | `S3://academics-cur-twl/attendances-list/metrics/` |
| Student        | `Students-list-Summarization`     | `S3://academics-cur-twl/students-list/metrics/`    |
| Instructor     | `Instructors-list-Summarization`  | `S3://academics-cur-twl/instructors-list/metrics/` |
| Program        | `Programs-list-Summarization`     | `S3://academics-cur-twl/programs-list/metrics/`    |
| Course         | `Courses-list-Summarization`      | `S3://academics-cur-twl/courses-list/metrics/`     |

---



---

## 🔍 Visual Architecture

![Architecture Diagram](https://github.com/yourusername/yourrepo/blob/main/architecture.png)

---

## 📃 References
- AWS Academy. (2022). *AWS Academy Cloud Foundations*. Retrieved from https://awsacademy.instructure.com/courses/106917/modules

---

## 👤 Author
**Your Name**  
Email: your.email@example.com  
GitHub: [yourusername](https://github.com/yourusername)

---


