## AWS-EC2 Tagging the Owner:

Lambda Function:
Create a Lambda function with Python runtime. 
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/2b46a430-5ec3-4f41-9886-a2d2d0a27d71)

We create this test event so that when we run this we get a cloud watch log group created.
![[Pasted image 20240113121233.png]]

CloudTrail:
Now create a CloudTrail give it a name and disable the KMS encryption.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/944f1b95-44eb-4b3e-93fb-88354b7fe15e)

Enable the CloudWatch logs and create a role for CloudWatch CloudTrailRoleForCloudWatchLogs_774721023955-da4d4d0d (this format is visible in the placeholder)
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/f0675331-5444-44de-866d-b8aa6d91cc7c)

Make sure the default settings are unchanged.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/2b97f367-e0ab-4897-8544-233ec8bd9e64)

Since all the settings look good we can create the Trail.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/68da4188-9fc5-44a1-a328-d5e7a0b12ccf)

EventBridge:
Give a name to the event
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/e434f240-9d86-4d40-9526-63308b200970)

In step - 2 select the event pattern:
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/c3eb28ac-b453-4827-9100-9ce147aedf3d)

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/14d060d1-844c-48f4-a31a-615b71ff917e)
Select the specific operation and give "RunInstances" because that is the keyword EventBridge will recognize. (Runinstances shown in the image is incorrect, it should be RunInstances)

Our target is Lambda function and select the function that we created in Lambda. Next step only review and create.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/115d9e7d-f2a6-4374-8590-d556029574fb)

Once the Event is created, when we enter Lambda, we can see that the Trigger Event is created. 
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/843818d6-193f-44b8-9658-e1aa403cf0d9)

Lambda Function Boto3:
[https://boto3.amazonaws.com/v1/documentation/api/latest/index.html]
Check on this documentation to get the desired code.
Now in the documentation -> go to Amazon EC2 Example -> and get the code to configure EC2.
```python
import boto3
ec2 = boto3.client('ec2')
```

```python
def lambda_handler(event, context):
#the event parameter gets the data from the event that triggered the lambda.
```

```python
import json
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    print(event)

```
this boto3 code will give you the data when we launch an Ec2 instance.

Once the code is type click **deploy.**

