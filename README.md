# lambda-scala-seed-template
Application to create AWS Lambda based on Scala with API Gateway Trigger and SNS/SQS Destination

This application is a simple implementation using serverless library to jumpstart development using Scala and show how AWS Lambda can be leveraged along with SNS/SQS and HTTP Api gateway to create a Event sourced application. 


#Set up 

Download and install from Serverless framework from Serverless.com. This will ease creation of configuration scripts for AWS Lambda

In order to quick start, we use project seed template "aws-scala-sbt" 

> serverless create --template aws-scala-sbt

Note : Since we are using aws-lambda-scala library to wrap Request and Response objects, we  only keep the Handler.scala file. Rest of the files are deleted. 

Update the build.sbt file with the following library dependency
> libraryDependencies += "io.github.mkotsur" %% "aws-lambda-scala" % "0.2.0"

#Building
Build the project 
> sbt assembly

#Deploying
Update the severless.yml file
```
functions:
    app:
     handler: app.ApiGatewayScalaHandler::handle
   events:
      - http:
          path: "lambda-event-app-api"
          method: post
          async: true
```

#Deploy the project 

> serverless deploy

#Running

As Synchronous process from command line (Might give erroneous response)
> serverless invoke --function app 

As Asynchronous process from command line (Might give erroneous response)
> serverless invoke --function app --type Event --data "{\"item\":\"banana\"}"

As Test event from Lambda page set the following event body
```
{
  "requestContext": {},
  "httpMethod": "GET",
  "path": "/lambda",
  "queryStringParameters": {
  },
  "headers": {
  },
  "body": "{\"item\":\"banana\"}"
 }
```

As API Gateway POST Async request
> set Integration Request "Use Lambda Proxy Integration" to false

> set Http Header "X-Amz-Invocation-Type" to 'Event'

> set Request Body passthrough to When no template matches the request Content-Type header  with no templates available.

As API Gateway POST Sync request
> set Integration Request "Use Lambda Proxy Integration" to true

> Unset Http Header "X-Amz-Invocation-Type" to 'Event'




