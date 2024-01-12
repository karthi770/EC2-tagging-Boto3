## AWS-EC2 Tagging the Owner:

Lambda Function:
Create a Lambda function with Python runtime. 
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/2b46a430-5ec3-4f41-9886-a2d2d0a27d71)

CloudTrail:
Now create a CloudTrail give it a name and disable the KMS encryption.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/944f1b95-44eb-4b3e-93fb-88354b7fe15e)

Enable the CloudWatch logs and create a role for CloudWatch 
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/10e4f005-8850-4823-af07-9b99aefaf06f)

Make sure the default settings are unchanged.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/2b97f367-e0ab-4897-8544-233ec8bd9e64)

Since all the settings look good we can create the Trail.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/68da4188-9fc5-44a1-a328-d5e7a0b12ccf)

EventBridge:
Give a name to the event
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/e434f240-9d86-4d40-9526-63308b200970)

In step - 2 select the event pattern:
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/c3eb28ac-b453-4827-9100-9ce147aedf3d)

Select the specific operation and give "Runinstances" because that is the keyword EventBridge will recognize.
![image](https://github.com/karthi770/EC2-tagging-Boto3/assets/102706119/ea6ffb7a-af7b-4083-98c3-19d282532230)

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



