# Table of Contents
1. Introduction

2. ER Diagram

3. Data pipeline

4. Final Result

5. Future Work

6. Requirements


# Introduction

**ETL pipeline for customer behaviour predict**

<p>This is a project completed in 4 weeks during the JR Academy Data Engineering program(Australia, Term 9). 
The goal of this project is to provide processed data for data scientists to predict customers whether reorder the products. 

In this project, I built an automation pipeline which mainly based on AWS and runs on a daily base. Also, Web developers could interact with results via API to enhance business strategies.  </p>

# ER Diagram


![ER Diagram](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/ER%20Diagram.png)

* Aisles: stores aisles’s data.
* Departments: stores a list of products departments categories.
* Products: stores a list of products.
* Orders: stores sales orders placed by customers.
* OrdersProduct: stores order line items for each order.

# Data Pipeline

Data Pipeline Overview

![data pipeline image](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/data%20pipeline.jpg)


* Step by Step
  * Step1: Assume the raw CSV files have been uploaded to an S3/input bucket every day.
  * Step2: AWS crawler crawl multiple data sources in S3/input via a single run. Upon completion, the crawler creates or updates tables in the AWS Data Catalog. Glue Crawler uses Athena quire data to get the order_products_prior table.
  * Step3: Set an EventBridge rule(you may want to check [crontab](https://crontab.guru/)) scheduled to run every day,  triggers 4 parallel DataBrew jobs. Load four parquet features files(transformed data) into S3/feature_db bucket.
          
  * Step4: Develop and test glue job codes in dev endpoints.
  * Step5: Set another EventBridge rule, scheduled to run every day after databrew job, triggers glue job and load repartition files into one parquet file. Load parquet file(transformed data) into S3 output bucket as result table.
  * Step6: Once the result table is loaded into the sandbox, data scientists will get a notification from AWS SNS, and then they can use Sagemaker for Machine Learning. 
  * Step7: Endpoint users will interact with the API gateway, the public web page will host on AWS S3, so users could access the result from anywhere.

 * Some thoughts 
   * __AWS S3__ for storage data
      * Raw Data/Staging Zone -> aisles, departments, order_products, orders, products tables
      * Curated Data Zone -> order_prodcuts_prior
      * Analytics Sandbox -> result_table  
    * __Athena__ for compute
    * __Glue DataBrew/Step functions__ for feature engineering/Orchestration
    * __Lambada Functions__ for automation processing
    * __Glue(Spark)__ for Big Data ETL processing
    * __SageMaker__ for Developmet and test Enviorment/ Model Endpoint
    * __API gate way__ for interact with endpoint users
    * __Parquet__ for better performance and lower cost


 # Final Result

 ![Final Result](https://github.com/richa-beep/ETL-pipelin-for-customer-behavior-predict/blob/main/static/Final%20Result.png)

As you can see, after we input the customer's feature data and submit it, we will get the result showing us the customer is unlikely/likely to buy this product.

 # Future Work/Enhancement

* Partitioning data
  * Partition by seasons – purchase intention for hot pot products in blazing summer.
  * Partition by department – accurately presents the products on the ‘suggestions for you’ section on the different catalogue pages.
  * Partition by age - purchase intention for e-cigarettes between young and old generations may vary a lot.

* Staffing Strategy
   * Involve with DevOps, Full Stack Developer to build the interface more user friendly, Data Analyst to build tableau dashboard to make the project more versatile and make it a parallel task.


 # Requirements

 * Language
    * Bash
    * Python 3.8
    * Pandas
    * Boto3
    * SQL

* Technologies
   * AWS S3, Athena, Crawker, Glue, Lambada, Eventbridge, SageMaker, API rest, SQS
   * Spark
   * Juypter Notebook

* Data Source Files  
https://drive.google.com/file/d/1pNNVQxAnhbOvvAagUoTsvk-pPjSgr4GZ/view






 
