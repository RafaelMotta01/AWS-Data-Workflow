# AWS-Data-Workflow

# Creating a workflow to proccess new data added to our source bucket

In this project the idea is to create a workflow that at first will wait for new data to be uploaded in the S3 source bucket and after the event is trigged it will check if the schema is the correct one we expect.
After that check it will start a series of transformations and aggregations in AWS Glue before sending the clean data to our target bucket. 

![Diagrama_project2](https://github.com/user-attachments/assets/cdf30ef0-637f-420f-a61e-39e192e67cb3)

# Description

The overall process consist of creating a source bucket, defining a rule in amazon event bridge that will trigger and AWS step functions state machine.
Inside AWS step functions we have our lambda function that will access the source bucket and raw data and validade if the columns are what we expect.
If the validation return a "Success" it will start an AWS Glue Workflow that will perform few aggregations and change some data formats.
In the end the data is send to a second bucket that is our target for process datasets.

After that we can use access our target bucket with tools like Amazon Athena to query the data.

# Note

This project uses a very small dataset so we can execute and validate each step of the process without incurring many costs on those AWS services.
