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





eksctl create iamserviceaccount \
  --cluster=iti-eks \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name "AmazonEKSLoadBalancerControllerRole" \
  --attach-policy-arn=arn:aws:iam::052124447099:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve




eksctl utils associate-iam-oidc-provider \
    --region "us-east-1" \
    --cluster "iti-eks" \
    --approve


  helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=iti-eks \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set image.repository=602401143452.dkr.ecr.region-code.amazonaws.com/amazon/aws-load-balancer-controller


 602401143452.dkr.ecr.us-east-1.amazonaws.com/amazon/aws-load-balancer-controller:v2.4.2

 --set image.repository=602401143452.dkr.ecr.region-code.amazonaws.com/amazon/aws-load-balancer-
 
 aws eks --region us-east-1 update-kubeconfig --name iti-eks


 eksctl create iamserviceaccount --cluster=iti-eks --namespace=kube-system --name=aws-load-balancer-controller --attach-policy-arn=arn:aws:iam::052124447099:policy/AWSLoadBalancerControllerIAMPolicy --override-existing-serviceaccounts --approve



onal.json
{
    "Policy": {
        "PolicyName": "AWSLoadBalancerControllerAdditionalIAMPolicy",
        "PolicyId": "ANPAQYIW36F575JWDOGYW",
        "Arn": "arn:aws:iam::052124447099:policy/AWSLoadBalancerControllerAdditionalIAMPolicy",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2022-07-03T11:48:15+00:00",
        "UpdateDate": "2022-07-03T11:48:15+00:00"
    }
}


aws iam attach-role-policy --role-name AmazonEKSLoadBalancerControllerRole  --policy-arn arn:aws:iam::052124447099:policy/AWSLoadBalancerControllerAdditionalIAMPolicy


cat <<EOF > nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
EOF