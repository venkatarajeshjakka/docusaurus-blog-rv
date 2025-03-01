---
title: AWS Lambda
description: "Introduction AWS Services : Lambda"
sidebar_label: "Lambda"
sidebar_position: 2
---

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers, creating workload-aware cluster scaling logic, maintaining event integrations, or managing runtimes.

These applications can have different triggers to invoke them, including HTTP API calls.

Embracing services like this which requires no infrastructure maintenance from an application developers side, is often termed as Serverless

## Tooling Support

The AWS Toolkit makes it easier to develop, debug and deploy (you should be looking to automate this) applications to AWS. There is an AWS Toolkit version for most of the IDE's out there.

The IDE's I commonly use for .NET development are `Visual Studio`, `Visual Studio Code` and `JetBrains Rider`, and toolkit is available for all of those.

Once installed and setup, you can start creating different project types from the available templates available.

## Setting Up First Project

Let's create a new project using the 'AWS Lambda Project (.NET Core - C#)' template option.

Selecting the option from the template list prompts you to choose the AWS Blueprint.

Blueprints contain some code (to get started) based on the type of trigger used to invoke the AWS Lambda function

The project created has one class file, `Function.cs`, which contains the code to run when the AWS Lambda function gets invoked.

The default `Function` class looks like below, with a method `FunctionHandler` that takes an `ILambdaContext` instance.

```csharp
 [LambdaFunction]
    [RestApi(LambdaHttpMethod.Get, "/")]
    public APIGatewayProxyResponse Get(APIGatewayProxyRequest request, ILambdaContext context)
    {
        context.Logger.LogLine("Get Request\n");
        var response = new APIGatewayProxyResponse()
        {
            StatusCode = (int)HttpStatusCode.OK,
            Body = "Hello Aws Serverless",
            Headers = new Dictionary<string, string>()
            {
                { "Content-Type", "text/plain" }
            }
        };

        return response;
    }

```

**Note:** : `AWSLambdaBasicExecutionRole`, which grants permissions to write to CloudWatch logs.

> The runtime config contains the information about the dotnet version and helps the AWS Lambda configure the runtime environment.

### Lambda Internals

The runtime provides a language-specific environment that runs in an execution environment. It relays invocation events, context information and responses between Lambda and the Function. You can use Lambda provided runtimes or custom runtimes.

Lambda supports out-of-the-box runtime for popular languages. You can also use custom runtime to use any other languages. For a .zip file package, configure to use a built-in runtime using the configuration file. When code is a container image, include runtime when you build the image.

The Amazon.Lambda.RuntimeSupport library helps to build custom runtimes for .NET .
