# 🚀 Automate EC2 Instance Creation via API Gateway + AWS Lambda + Boto3

This project demonstrates how to programmatically launch EC2 instances by triggering a Lambda function via an API Gateway endpoint. This serverless automation setup can help streamline infrastructure provisioning for DevOps, test environments, and dynamic scaling.

## 🔧 Technologies Used
- AWS Lambda (Python 3.x)
- Amazon API Gateway (REST API)
- Amazon EC2 (t2.micro)
- IAM Roles with EC2 permissions
- Boto3 (AWS SDK for Python)

## 📋 Prerequisites
- AWS account with IAM role having:
  - `AmazonEC2FullAccess`
  - `AWSLambdaBasicExecutionRole`
- Lambda function with attached role
- API Gateway configured with Lambda integration

## 📌 Lambda Function Code (Python)
```python
import json
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2', region_name='ap-south-1')
    
    try:
        instance = ec2.run_instances(
            ImageId='ami-002f6e91abff6eb96',
            InstanceType='t2.micro',
            MinCount=1,
            MaxCount=1
        )
        
        return {
            'statusCode': 200,
            'body': json.dumps({
                'message': 'EC2 instance created',
                'InstanceId': instance['Instances'][0]['InstanceId']
            })
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
