# AWS-Data-Workflow
AWS Project

# Creating a workflow to proccess new data added to our source bucket

In this project the idea is to create a workflow that at first will wait for new data to be uploaded in the S3 source bucket and after the event is trigged it will check if the schema is the correct one we expect.
After that check it will start a series of transformations and aggregations in AWS Glue before sending the clean data to our target bucket. 



# Description
