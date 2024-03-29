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

> [!NOTE]
this boto3 code will give you the data when we launch an Ec2 instance.

Once the code is typed click ==**deploy.**==

Now create an instance and took into CloudWatch, you could see a log group thats been created and a huge JSON format can be seen in the log. 
This JSON is the result of `print(event)` in the code. 
![AWS](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/110a8102-f9d8-4cef-977c-72369e6c7da3)

We need 2 data from this output, one is the username and the other is instance id. So we create 2 variables and we extract those data from the JSON.

```python
import json
import boto3

ec2 = boto3.client('ec2')

def lambda_handler(event, context):
    print(event)
    user = event['detail']['userIdentity']['accountId']
    instanceId = event['detail']['responseElements']['instancesSet']['items'][0]['instanceId']
    ec2.create_tags(
        Resources=[
            instanceId
        ],
        Tags=[
            {
                'Key': 'Owner',
                'Value': user
            },
        ]
    )
    return

```

>[!TIP]
>The syntax for create tags can be seen in the boto3 documentation.

The above boto3 code shall tag the ec2 instance that was created by an user but ==the permission for Lambda function to access the ec2 instance needs to be given.==

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/6097eb43-c2e8-4ad1-b286-ea946e0aae6a)

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/b5072372-7862-4f07-9860-b23ec14f9ae5)

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/5d975309-d46e-48ad-a006-cdba29c89b22)

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/372d5ade-f5f8-4093-b4b5-372f0f79728b)

![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/fef72b01-f2dd-4bb1-8b01-610f3948810f)
From the above image we can see the owner seems to be account id of the user who created the ec2 instance.
