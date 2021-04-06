# Local Dev Environment

## Prerequistes: 

1. Install npm
2. Run `npm install -g serverless`
3. Install Java if not installed. 
4. Install Maven if not installed.


## 1. Setting Up Dynamo DB Locally

We will be using serverless-dynamodb-local npm module in order 
for us to work with DynamoDB locally. 

### 1.1 Steps

1. Run `npm install -g serverless-dynamodb-local`
2. Run `sls dynamodb install` ( Run this command if doing a fresh setup) - This command will download and install dynamodb local jar 
3. Run `sls dynamodb start` in a new terminal - This command will start the dynamoDB locally @ port 8000. Hit http://localhost:8000/shell to access the dynamoDB shell 
**We need not start the dynamodb if we are going to start the sls offline**

## 2. Running serverless locally

We are going to run our serverless service locally without needing to 
deploy our functions to cloud again and again for the development purposes

### 2.1 Steps

Run the followning commands to setup your local dev env

1. Run `npm install serverless-offline -g`
2. Run `mvn clean install`
3. Run `sls offline start`  in a new/seperate terminal - This will deploy your serverless services locally. 

**NOTE :** 
1. Right now the dynamoDB Adapter is configured to run locally, but this would be 
updated to later to interact with cloud DB at later stage. 
2. You'll need to restart the serverless-offline plugin if you modify your serverless.yml or any of the default velocity template files.

## 3. Process for local dev [May improve overtime]

1. Override ApiGatewayResponse in a new Class in order for you to create a new lambda and 
update the serverless.yml file to add the endpoint at which it should be invoked. 

2. To add a new table to dynamoDB, please see the resources section 

```
resources:
  Resources:
    ProductsDynamoDBTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: ${self:custom.productsTableName}
        AttributeDefinitions:
          - AttributeName: id
            AttributeType: S
          - AttributeName: name
            AttributeType: S
        KeySchema:
          - AttributeName: id
            KeyType: HASH
          - AttributeName: name
            KeyType: RANGE
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
```

you can refer to this template and update the serverless.yml to create/modify your existing tables
or create new ones.

3. Whenever you make changes to the java files, you would need to run `mvn clean install` to build a 
new jar that can be deployed on sls-local