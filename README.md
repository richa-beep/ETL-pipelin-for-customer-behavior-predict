# Table of Contents
1. Introduction

2. Data pipeline

3. Requirements

4. Enviroment Set UP

5. Repository Structure and Run Instructions


# Introduction

**ETL pipleline for customer behavior predict**

<p>This is a project completed in 4 weeks during the JR Academy Data Engineering program(Australia,Term 9). 
The goal of this project is to provide processed data for data scientist predict customer wheather reorder the prodcuts. 

In this project, I built an auomation pipeline which mainly based on AWS run on daily base. Also Web developers could interactive with results via API to enhance business strategies.  </p>

# Data Pipeline

Data Pipleline Overview

![data pipeline image](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/54e7c6a05b7fd20b36deda955c54cc768745b0c9/static/photo_2022-04-16_19-25-05.jpg)

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
    


* Step by Step
  * Step1: Assume the raw CSV files been uploaded to an S3/input bucket every day.
  * Step2: AWS crawler crawl multiple data sources in S3/inout via a single run. Upon completion, the crawler creates or updates tables in the AWS Data Catalog. Glue Crawler use Athena quire data to get order_products_prior table.
  * Step3: Set an EventBridge rule(you may want to check [crontab](https://crontab.guru/)) scheduled to run every day,triggers four parallel DataBrew jobs 
          Load four parquet features files(transformed data) into S3 feature_db bucket.
          (parquet is more efficient in terms of storage and performance)
  * Step4: dev and test glue job codes in dev endpoints
  * Step5: Another EventBridge rule, scheduled to run every day at 9am,triggers glue job and load repartition files into one parquet. Load  parquet file(transformed data) into S3 output bucket.
  * Step6: After the output data is written, data scientists can use Sagemaker for Machine Learning.
 






 
