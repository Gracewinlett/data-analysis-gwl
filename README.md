Exploratory Data Analysis
Project Description: 
This project focuses on performing exploratory data summarization for the Academics Department's Attendance Procedure. The aim is to extract structured academic datasets stored in the S3 bucket which are already cleaned and profiled using DataBrew and then load them into AWS Glue Crawlers to create a Data Catalog. From there, AWS Glue ETL jobs are used to generate summarized datasets for reporting and performance insights.
Project Title: Exploratory Data Analysis using Summarization on Attendance Datasets for Academic Department
Objective: To ingest cleaned data from the S3 bucket, catalog them using AWS Glue Crawler, and generate summarized attendance insights using AWS Glue ETL. This allows for early-stage exploration of key attendance metrics such as student participation trends and attendance patterns across programs, instructors, and courses.
Dataset: The Attendance datasets consist of below tables
•	Attendance List
•	Student List
•	Instructor List
•	Program List
•	Course List
These datasets were pre-processed and cleaned before being used in this summarization step.
Methodology:
Below figure shows that overall architecture of the Data Analysis Platform using AWS for Academics Office.
 


1.	Data Cataloging
We assume that raw Datasets in S3 bucket (Academics-raw) are already profiled, cleaned and stored in transformed buckets (Academics-trf). Those datasets will be registered into the Glue Data Catalog using AWS Glue Crawler. Create academics-crw and do data catalog. 
 

After running the crawler then catalog tables will be shown under Data Catalog.
 
Verified the cataloged data in AWS Glue and checked tables’ schemas.
2.	Data Summarization
Glue ETL jobs are then used to transform and summarize the attendance data. Summarized output is stored in the curated S3 bucket, partitioned by report date, and can be used for descriptive and visual analysis.

Created an AWS Glue ETL job “Attendances-list-Summarization" as below.
•	Extract “Attendance List” data from data catalog
•	Transform
o	Group by
	attendancedate
	courseid
	instructorid
	attendedstatus
o	Aggregate
	studentid countDistinct
•	Transform - add Report_Date to summary
•	Transform - convert to Report_Date_LTZ
•	Transform - prepare for load, remove Report_Date column
•	Data target - load to system (parquet format, snappy compression)
•	Transform - convert to one file
•	Data target - load to user (csv)
 
Created an AWS Glue ETL job “Students-list-Summarization" as below.
•	Extract “Student List” data from data catalog
•	Transform
o	Group by
	programid
	status
o	Aggregate
	studentid count
•	Transform - add Report_Date to summary
•	Transform - convert to Report_Date_LTZ
•	Transform - prepare for load, remove Report_Date column
•	Data target - load to system (parquet format, snappy compression)
•	Transform - convert to one file
•	Data target - load to user (csv)
 
Created an AWS Glue ETL job “Instructors-list-Summarization" as below.
•	Extract “Instructor List” data from data catalog
•	Transform
o	Group by
	department
	status
o	Aggregate
	instructorname count
•	Transform - add Report_Date to summary
•	Transform - convert to Report_Date_LTZ
•	Transform - prepare for load, remove Report_Date column
•	Data target - load to system (parquet format, snappy compression)
•	Transform - convert to one file
•	Data target - load to user (csv)
 
Created an AWS Glue ETL job “Programs-list-Summarization" as below.
•	Extract “Program List” data from data catalog
•	Transform
o	Group by
	Department
	programdirector
	programlength
o	Aggregate
	programname countDistinct
	creditsrequired avg
	programlength avg
•	Transform - add Report_Date to summary
•	Transform - convert to Report_Date_LTZ
•	Transform - prepare for load, remove Report_Date column
•	Data target - load to system (parquet format, snappy compression)
•	Transform - convert to one file
•	Data target - load to user (csv)
 
Created an AWS Glue ETL job “Courses-list-Summarization" as below.
•	Extract “Course List” data from data catalog
•	Transform
o	Group by
	coursename
	semester
o	Aggregate
	courseid count
•	Transform - add Report_Date to summary
•	Transform - convert to Report_Date_LTZ
•	Transform - prepare for load, remove Report_Date column
•	Data target - load to system (parquet format, snappy compression)
•	Transform - convert to one file
•	Data target - load to user (csv)
 
3.	Data Storage
Data generated from ETL summarization jobs will be stored in curated bucket (Academics-cur). System friendly files (parquet format with snappy compression) will be stored under the system folders of respective data list folders as shown in the screenshots below.

 
 
 
 
 
User friendly files (csv format) will be stored under user folders of respective data list folders as shown in the screenshots below.
 


 


 


 


 

4.	Monitoring and log
The ETL jobs runs history information will be available under job run monitoring as shown in below.
 





Tools and Technologies:
•	Amazon S3 – transformed and curated data storage
•	AWS Glue Crawlers – Cataloging structured datasets
•	AWS Glue ETL – Data transformation and summarization
•	AWS Glue Data Catalog – Central metadata repository
•	Amazon Athena – Future querying and analysis (not in scope here)
Deliverables:
•	Registered datasets in the AWS Glue Data Catalog
•	Summarized attendance datasets generated through ETL jobs
•	Output files stored in system and user folders under curated S3 bucket

