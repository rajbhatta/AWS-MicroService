# How to create staging server in API Gateway #
## Add code section as defined below in template.yaml to define environment variable ##
```js
Parameters:
  Environment:
    Type: String
```

```js
HelloWorldApi:
    Description: "API Gateway endpoint URL for Prod stage for Hello World function"
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/${Environment}/hello/"
```

## Complete template.yaml ##
```js
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: >
  lambda-java11-xray

  Sample SAM Template for lambda-java11-xray

Globals:
  Function:
    Timeout: 20

Parameters:
  Environment:
    Type: String
    

Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: HelloWorldFunction
      Handler: helloworld.App::handleRequest
      Runtime: java11
      Architectures:
        - x86_64
      MemorySize: 512
      Environment:
        Variables:
          PARAM1: VALUE
      Events:
        HelloWorld:
          Type: Api 
          Properties:
            Path: /hello
            Method: get
      
      Tracing: Active
      Policies:
        - Statement:
          - Sid: LAMBDAS3READPOLICY
            Effect: Allow
            Action:
            - s3:Get*
            - s3:List*
            - s3-object-lambda:Get*
            - s3-object-lambda:List*
            Resource: '*'

 
Outputs:
  HelloWorldApi:
    Description: "API Gateway endpoint URL for Prod stage for Hello World function"
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/${Environment}/hello/"

  HelloWorldFunction:
    Description: "Hello World Lambda Function ARN"
    Value: !GetAtt HelloWorldFunction.Arn

  HelloWorldFunctionIamRole:
    Description: "Implicit IAM Role created for Hello World function"
    Value: !GetAtt HelloWorldFunctionRole.Arn

```

# Output #
<img src="img/img1.png"/>