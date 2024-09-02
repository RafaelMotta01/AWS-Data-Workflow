# AWS-Data-Workflow

# Creating a workflow to proccess new data added to our source bucket

In this project the idea is to create a workflow that at first will wait for new data to be uploaded in the S3 source bucket and after the event is trigged it will check if the schema is the correct one we expect.
After that check it will start a series of transformations and aggregations in AWS Glue before sending the clean data to our target bucket. 

![Diagrama_project2](https://github.com/user-attachments/assets/cdf30ef0-637f-420f-a61e-39e192e67cb3)

# Description

The overall process consist of creating a source bucket, defining a rule in amazon event bridge that will trigger a AWS step functions state machine.
Inside AWS step functions we have our lambda function that will access the source bucket and raw data and validade if the columns are what we expect.
If the validation return a "Success" it will start an AWS Glue Workflow that will perform few aggregations, change some data formats and convert the file to parquet.
In the end the data is send to a second bucket that is our target for process datasets.

After that we can use access our target bucket with tools like Amazon Athena to query the data.

# Note

This project uses a very small dataset so we can execute and validate each step of the process without incurring many costs on those AWS services.

# Step by step
1. **Create both buckets**, (preferible in the same AWS Region) and enable the source bucket to send notifications for Amazon Event Bridge on the properties setting. After that upload the file on the source S3 bucket.
2. **Create the AWS Glue ETL Job**, go to visual ETL, select S3 as a source and chose our bucket and infer the schema to recognize our file. After that add the transformations you want, like change the schema or aggregations. To finish this part select target and chose S3 again and select the correct bucket as destiantions and parquet as the file format. Recommendable to reduce the number of worker in "job details" tab to reduce the costs.
3. **Create the AWS Lambda function**, using python for this example (important note before deploy to change the name of the bucket on the code). After deploying the lambda function we have to grant permission for the role that is executing the function to access the S3 bucket. (attach the S3 policy)
4. **Create the AWS Step Functions State Machine**, drag and drop the AWS Lambda Invoke Function and choose our AWS LAmbda Function created on step 3. Select a choice state to drag and drop and set the rule of "status" equal to "success". Last search for "Glue StartJobRun" to drag an drop and set our job name created on step 2.
5. **Create our Rule in Event Bridge**, define the new rule to run on event pattern and select our source as S3 and event type choose "Amazon S3 Event Notification" and as a specific event choose "Object Created". After that the target of our rule should be "Step Functions state machine" created on step 4.
6. **Upload the file to the bucket for testing.**
