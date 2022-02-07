# Create a fanned out architecture for a aws serverless app given below
 Services used :
 1. s3 bucket 
 2. sns service 
 3. sqs service 
 4. a lambda trigger
 5. dynamodb database
 
 overview : when we upload a file to s3 bucket then a event will trigger the sns service then a subscription of that sns will be a sqs que then a msg will go to sqs then from sqs a lambda will be trigger and then the code logic in lambda that read the uploaded csv file and fetch the data and upload the data inside the created table in dynamodb
 
 DIAGRAM: 
![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/awsfannedout.png)

## Step 1 :  CREATE A S3 BUCKET 
### 1. Go to AWS console and on s3  dashboard create a s3 bucket

![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/aws1.PNG)

## Step 2 : CREATE A SNS TOPIC 
### 1. Go to the SNS service console and create a topic 
### 2. Select topic type Standard,give your topic a name 
### 3. On access Policy choose method `Basic` select every one in radio button for `who can publish message` and `who can who can subscribe`
### 4. Select a delivery status for checking Logs in cloudwatch  select a service Role for that (Create a IAM service role and give it Permission of S3 Full Access,SNSfullaccess,SQS Full Access,Lambda Execute Full Access,Dynamodb Full Access,CloudWatch FullAccess)
### 5. Create the Topic

## Step 3 : Create a SQS Queue and subscribe it to the SNS Topic that we just Created.
### 1. Go to the SQS service console and create a Queue 
### 2. Select Que type Standard,give your Que a name 
### 3. On access Policy choose method `Basic`, select the access type that you want to allow.
### 4. Create the Que
### 5. Select a SNS Subsciption for the Que and subscribe your que with the SNS Topic
![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/awssns.PNG)


## Step 4 : Create a Lambda Function and Trigger it with SQS Que you just Created.
### 1. Go to the Lambda service console and create a Lambda Function 
### 2. Select `Author from scratch`,give your function a name,Runtime Python
### 3. In Permissions Select a Role (Your Role Should have all the Nescessary Permissions Choose the role that we created in sns).
### 4. Create the Lambda Function
### 5. Choose the add Trigger Option and choose SQS,select our sqs que From There so that this lambda Function can be triggered from this SQS que
![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/New%20folder/aws%20lambda.PNG)

### Code For Lambda Function
```python
import json
import boto3
s3=boto3.client("s3")
dynamodb=boto3.resource('dynamodb')
table =dynamodb.Table('employee')
def lambda_handler(event, context):
    # TODO implement
    print(event)
    for record in event['Records']:
      
        jsonmaybe=(record["body"])
        jsonmaybe=json.loads(jsonmaybe)
        
        print(jsonmaybe)
        jsonmaybe1=json.loads(jsonmaybe['Message'])
        bucket_name = jsonmaybe1["Records"][0]["s3"]["bucket"]["name"]
        print(bucket_name)
        bucket_key=jsonmaybe1["Records"][0]["s3"]["object"]["key"]
        print(bucket_key)
    bucket_response=s3.get_object(Bucket=bucket_name,Key=bucket_key)    
    data=bucket_response["Body"].read().decode("utf-8")
    print(data,"bucket data")
    employees=data.split("\n")
    for emp in employees:
        print(emp,"each record")
        emp_data=emp.split(",")
        try:
            table.put_item(
            Item={
                "id" :emp_data[0],
                "name" : emp_data[1],
                "location" : emp_data[2]
            }    
            )
            print("item inserted")
        except Exception as e:
            print("End of file")
    return {
        
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }

```

## Step 5: Go to the Dynamodb Console and create a Table 'empolyee' choose a primary key id and datatype "string"

![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/New%20folder/awsdynamodb.PNG)

*Check this Architecture by uploading a file the flow is properly connected or not and check the Logs on CloudWatch*  


Choose your CSV file Structure Like this as per the given Lambda function Code.

| id            | name          |location|
| ------------- |:-------------:| -----: |
| 1             | ravi          | Bihar  |
| 2             | Kisan         | Bihar  |

References:
https://www.youtube.com/watch?v=lRD_oHoZHlI
https://www.youtube.com/watch?v=8BEwZnUIZfw
