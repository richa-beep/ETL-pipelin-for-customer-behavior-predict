# Table of Contents
1. Introduction

2. ER Diagram

3. Data pipeline

4. Final Result

5. Future Work

6. Requirements


# Introduction

**ETL pipleline for customer behavior predict**

<p>This is a project completed in 4 weeks during the JR Academy Data Engineering program(Australia,Term 9). 
The goal of this project is to provide processed data for data scientist predict customer wheather reorder the prodcuts. 

In this project, I built an auomation pipeline which mainly based on AWS run on daily base. Also Web developers could interactive with results via API to enhance business strategies.  </p>

# ER Diagram

Draw a ER diagram help us better understaning the data source.

![ER Diagram](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/ER%20Diagram.png)

* Aisles: stores aisles’s data.
* Departments: stores a list of products departments categories.
* Products: stores a list of products.
* Orders: stores sales orders placed by customers.
* OrdersProduct: stores order line items for each order.

# Data Pipeline

Data Pipleline Overview

![data pipeline image](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/data%20pipeline.jpg)

* Some thoughts 
   * __AWS S3__ for storage data
      * Raw Data/Staging Zone -> aisles, departments, order_products, orders, products tables
      * Curated Data Zone -> order_prodcuts_prior
      * Analytics Sandbox -> result_table  
    * __Athena__ for compute
    * __Glue DataBrew__ for feature engineering
    * __Lambada Functions__ for automation processing
    * __Glue(Spark)__ for Big Data ETL processing
    * __SageMaker__ for Developmet and test Enviorment/ Model Endpoint
    * __API gate way__ for interact with endpoint users
    * __Parquet__ for better performance and lower cost
    


* Step by Step
  * Step1: Assume the raw CSV files been uploaded to an S3/input bucket every day.
  * Step2: AWS crawler crawl multiple data sources in S3/inout via a single run. Upon completion, the crawler creates or updates tables in the AWS Data Catalog. Glue Crawler use Athena quire data to get order_products_prior table.
  * Step3: Set an EventBridge rule(you may want to check [crontab](https://crontab.guru/)) scheduled to run every day,  triggers 4 parallel DataBrew jobs. Load four parquet features files(transformed data) into S3/feature_db bucket.
          
  * Step4: Develop and test glue job codes in dev endpoints.
  * Step5: Set another EventBridge rule, scheduled to run every day after databrew job,triggers glue job and load repartition files into one parquet file. Load  parquet file(transformed data) into S3 output bucket as result table.
  * Step6: Onece result table loaded into sandbox, data scientists will get notification from AWS SNS, then they can use Sagemaker for Machine Learning. 
  * Step7: Endpont users will interact with API gateway, the Pulice page will host on AWS S3, so users could access data from anywhere.
 
 # Final Result

 ![Final Result](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/Final%20Result.png)

 # Future Work/Enhancement

 # Requirements

 * Language
    * Bash
    * Python 3.8
    * Pandas

* Technologies
   * AWS S3, Athena, Crawker, Glue, Lambada, Eventbridge, SageMaker, API rest, SQS
   * Spark

* Downlod Data Files  
https://drive.google.com/file/d/1pNNVQxAnhbOvvAagUoTsvk-pPjSgr4GZ/view






 
