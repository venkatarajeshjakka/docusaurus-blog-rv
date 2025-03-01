---
title: AWS DynamoDB
description: "Introduction AWS Services : DynamoDB"
sidebar_label: "DynamoDB"
sidebar_position: 3
---

DynamoDB is a cloud-hosted NoSQL database provided by Amazon Web Services (AWS). DynamoDB provides reliable performance, a managed experience, and a convenient API access to interact with it.

With DynamoDB, you can create database tables that can store and retrieve any amount of data and serve any level of request traffic. DynamoDB supports on-demand backups and can also enable point-in-time recovery of the data.

It also supports automatic expiry of items to reduce storage and associated cost.

DynamoDB is a fully managed database, which means you don't need to spin up server instances, software installations, or other maintenance tasks.

# Set Up DynamoDB

To create a Table to hold the Data, head off to the AWS Console and navigate to DynamoDB. Make sure you are in the appropriate Region where you want to create the Table.

> The core components in DynamoDB are `Tables`, `Items` and `Attributes`.

To create a table, use the 'Create table' button on the DynamoDB page. It prompts you to enter the Table Name and Primary key as shown below.

DynamoDB supports two different kinds of primary keys.

- Partition Key → A simple primary key, with just one attribute. Not two items can have the same partition key value
- Partition Key and Sort Key → A composite primary key composed of two attributes. One attribute is the partition key, and the other a sort key. It's possible for two items to have the same partition key; however, they must have different sort keys. The combination of them must be unique.

To add items to the Table from the portal, use the Create Item button.

## Set up App to Use DynamoDB

With the data set up in DynamoDB, let's update the Web API application to retrieve the data from DynamoDB.

To get started, let's first add the DynamoDB NuGet package. → `AWSSDK.DynamoDBv2`

The `IAmazonDynamoDB` provides the necessary methods to interact with DynamoDB. Update the `WeatherForecastController` constructor to take in the interface.

## Dependency Injection

With the Controller now using the `IAmazonDynamoDB` let's wire up the application to be able to dependency inject an implementation for it.

```csharp
var config = new AmazonDynamoDBConfig()
{
    RegionEndpoint = RegionEndpoint.APSouth1
};
var client = new AmazonDynamoDBClient(config);
builder.Services.Configure<DatabaseSettings>(builder.Configuration.GetSection(DatabaseSettings.KeyName));
builder.Services.AddSingleton<IAmazonDynamoDB>(client);
```

The above code creates an `AmazonDynamoDBClient` using the credentials and Region to access DynamoDB for your account. The `DynamoDBContext` requires only the `AmazonDynamoDBClient`.

## CRUD Operations

### GET

```csharp
    [HttpGet]
    public async Task<IActionResult> GetAsync(string city, string country)
    {
        var request = new GetItemRequest
        {
            TableName = _databaseSettings.Value.TableName,
            Key = new Dictionary<string, AttributeValue>()
            {
                ["city"] = new AttributeValue { S = city },
                ["country"] = new AttributeValue { S = country },
            }
        };

        var response = await _db.GetItemAsync(request);
        if (response.Item?.Count == 0)
        {
            return NotFound();
        }

        var itemAsDocument = Document.FromAttributeMap(response.Item);

        return Ok(JsonSerializer.Deserialize<Weather>(itemAsDocument.ToJson()));
    }

```

### POST

```csharp
    [HttpPost]
    public async Task<bool> Post(Weather weather)
    {
        var customerAsJson = JsonSerializer.Serialize(weather);

        var itemAsDocument = Document.FromJson(customerAsJson);

        var itemAsAttribute = itemAsDocument.ToAttributeMap();

        var createItemRequest = new PutItemRequest
        {
            TableName = _databaseSettings.Value.TableName,
            Item = itemAsAttribute
        };
        var response = await _db.PutItemAsync(createItemRequest);

        return response.HttpStatusCode == HttpStatusCode.OK;
    }
```

### DELETE

```csharp
    [HttpDelete]
    public async Task<bool> DeleteAsync(string city, string country)
    {
        var request = new DeleteItemRequest()
        {
            TableName = _databaseSettings.Value.TableName,
            Key = new Dictionary<string, AttributeValue>()
            {
                ["city"] = new AttributeValue { S = city },
                ["country"] = new AttributeValue { S = country },
            }
        };
        var response = await _db.DeleteItemAsync(request: request);

        return response.HttpStatusCode == HttpStatusCode.OK;
    }
```

## DynamoDB API Overview

The Amazon DynamoDB support provided in the AWS SDK for .NET is divided into three layers:

- Low-Level Interface → found under the namespaces Amazon. DynamoDB'and' Amazon.DynamoDB.Model. It is the API most closely related to the service model, with little overhead or helper functionality.
- Document Interface → found under the namespace `Amazon.DynamoDB.DocumentModel`. It's an API based around Table and Document classes.
- High-Level Interface → found under the namespace `Amazon.DynamoDB.DataModel`. Easiest to work with .NET classes and is what we used in the code samples above.
  Not all functionality is available via the High-Level interface, e.g., to create a new table. In these cases, we will need to fall back to the lower-level APIs.
