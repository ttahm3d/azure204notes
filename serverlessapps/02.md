# Creating serverless logic with Azure Functions

We can deploy our app as a serverless function using Azure Functions. Azure Functions offer automatic scaling, pay as you go and pay only for the time the function runs features. It falls under code first approach

## Serverless Compute

It can be imagined or thought as Function as a service. The business logic runs as a function. You don't have to provision the infrastructure. Functions provide scalability and you are charged only for the compute resources that you use. Azure offers two types of serverless compute

- Auzre logic apps
- Auzre Functions

## Azure Functions

Azure Functions is a serverless platfrom. It allows users to deploy their business logic without any infrastucture. Functions provides scalability and you are charged only for the resources used. You can write your function code in the language of your choice, including C#, F#, JavaScript, Python, and PowerShell Core

### Benefits of Serverless compute

- Automated scaling
- no infrastucture
- Charged for what resource you use - not for reserved time
- **Avoids over allocation of resources** - When we use Vm we need to configure for peak loads so that it handles the capacity, which is seldom reached. Most of the time it remains in minimal usage. We are paying for something we are not using. it handles scaling on its own
- **Stateless** - they are created or destroyed on demand. If state is required it can be stored in storage service
- **Event Driven** - They run in response to an event
- Flexibility if you want to move out of functions - they can also run on compute infras

### Drawbacks

- **Default timeout of 5 mins** - Maximum 10 mins. For HTTP triggers it is 2.5 mins. _Durable Functions_ run without any timeouts
- If Execution frequency is high, it is better off to use VM

**only one function app instance can be created every 10 seconds, for up to 200 total instances**. Each instance can server multiple connections

| Timeout       | Duration |
| ------------- | -------- |
| Default       | 5 mins   |
| Maximum       | 10 mins  |
| HTTP triggers | 2.5 mins |

## Creating a function App

Functions are hosted in execution context (environment sort of) called as function app. Function app logically groups the functions. We need to choose a service plan and a storage account (in case if we need to manage state of the function).

### Service plan

1. Consumption service plan: Default plan chosed when using serverless compute. Provides automatic scaling and pay for resources consumed features. Time out options as above

2. Azure App service plan: Avoids timeouts by running the function on a VM. Technically not a serverless plan but it is useful in case if the functions run very frequently.

### storage account

- Used for storing the logs and managing triggers
- In case of Consumption plan: it is here the code and config of function resides

## Running a function in function app on demand

Functions are event driven and they are run on triggers. One function should handle only on trigger. Azure supports triggers from Blob Storage, COSMOS DB, Event Grid, HTTP, Microsoft Graph Events, Queue Storage, Service bus and Timer triggers.

**Bindings** are declartive way of connecting data and services to your function. Bindings have capability to talk to different services. We need not include code to connect to data and manage connections in the function. Each binding has direction - read from input bindings and write to output bindings.

A trigger is special input binding that is capable of initiating execution. Bindings are defined in JSON format

```
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Eg. Think of a scenario where we have to trigger an event when a message comes in Queue storage and write to Azure Table. This can be implemented using an Azure Queue storage trigger and an Azure Table storage output binding.

## Create a function in portal

Azure provides serveral templates for common scenarios

- Quickstart Templates - You can select the trigger and Azure will write the code
- Custom function templates - Azure provides over 30 templates for creating functions

### Functions and Files

When the function is created using templates it creates two files

1. function.json - contains the configuration of the function
2. index.js - the code of the function

![sample function](https://docs.microsoft.com/en-us/learn/modules/create-serverless-logic-with-azure-functions/media/4-file-navigation.png)

## Testing Azure Functions

### Manual Testing

We can start the function using manual trigger. Eg. HTTP Trigger functions can be tested using cURL and POSTMAN

### Azure portal testing

You can click on Run and view the result of run

## Monitoring Functions and Logging

We can monitor the functions if Application Insights is Integrated

We can log custom logs by passing logging object to the log object

# Creating a Function and running

- Open the Azure Function
- Got to function and click on Code + Test
- you can add custom log using `context.log("custom message")`
- The function is secure from unauthorized persons
- By default authorization is handled at function level. Auth can be set to admin to use master key or anonymous to use no auth
- You need to pass the function key as Header `x-functions-key: <your-function-key>` or query sting parameter as `code`
- combine the cURL command or use the Test /Run option in portal
