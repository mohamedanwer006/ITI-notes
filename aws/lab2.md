# lab2 

```py
import json
import boto3
import urllib.parse
from botocore.exceptions import ClientError
from datetime import datetime

# Getting the current date and time
dt = datetime.now()

# getting the timestamp
ts = datetime.timestamp(dt)

def lambda_handler(event, context):
    print("---------event------------")
    print(event)
    
    source_bucket_name="source-bucket-mohamedanwar"
    target_bucket_name="target-buket-mohamedanwar"
    s3_client = boto3.client('s3')
    # s3 = boto3.resource('s3')
    
    key = urllib.parse.unquote_plus(event['Records'][0]['s3']['object']['key'])
    print("-------------key-----------")
    print(key)
    file_content = s3_client.get_object(
    Bucket=source_bucket_name, Key=key)["Body"].read()
    print("------------file_content----------------")
    print(file_content)
    
    response = s3_client.put_object(Body=file_content,
    Bucket=target_bucket_name,Key=key)
    
    print("------------add to dynamo db--------------")
    dynamo_client = boto3.client('dynamodb')
    
    data={"S":str(key),}
    key_data={"S":str(ts)}
    response = dynamo_client.put_item(
    TableName='s3_table',
    Item={"name":data,"id":key_data} )
   
    return {
        'statusCode': 200,
        'body': json.dumps('upload done')
    }

```

## test

![alt](./assets/Screenshot%20from%202022-07-02%2016-10-02.png)