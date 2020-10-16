# PetShopAWS

# Manual deployment of Lambda using AWS Client
**aws lambda create-function 
**--function-name oleks-cuckoo 
**--runtime python3.8 
**--role arn:aws:iam::698721136048:role/service-role/oleks-canary-lambda-role-f0bt2e9o  --> this role should be created beforehand
**--handler cuckoo.handler 
**--zip-file fileb://./package.zip 
**--profile dev-VPC**


# CloudWatch Triggers
You can create a trigger from AWS client
	**aws events put-rule --profile dev-VPC --name come_to-work --schedule-expression 'cron(15 * * * ? *)'
And then use the returned ARN to add to the lambda. Otherwise, use 
	**aws events list-rules --profile dev-VPC
To get all the existing events

Add permissions for lambda to access CloudWatch events - CloudWatch event has access to lambda
	**aws lambda add-permission --function-name oleks-cuckoo --statement-id 3 --action 'lambda:InvokeFunction' --principal events.amazonaws.com --source-arn arn:aws:events:us-east-1:698721136048:rule/pickup --profile dev-VPC

CloudWatch event will target Lambda
	**aws events put-targets --rule daily_tasks --targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:698721136048:function:oleks-cuckoo"

NOTE (!)
When working from Powershell, use --% to prevent parsing problems related to = char
	**aws --% events put-targets --rule DailyLambdaFunction --targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:123456789012:function:MyFunctionName"