# Choosing the Best service for our requirement

Busineses may have multiple processes. Each process will have certian steps that need to be implemented in a particular order. Business processes modeled in software are often called workflows. Azure provides

- Auzre Functions
- Auzre Logic apps
- Azure WebJobs
- Azure Power Automate

to implement these workflows.

All of these can accept an input, perform actions, check conditions and produce an output.

There are two approaches for implementing workflows - Design first and Code first approaches.

## Design First Approach

These are helpful in the situations where the flow is defined. There is certial idea how the control / data flows in the application. Both _Azure Logic Apps_ and _Auzre Power Automate_ fall under this category

### Azure Logic Apps

Logic Apps are helpfull to automate / integrate separate components of an application. Using the design-first approach, we can design the workflows that in-turn are part of a large workflow

Can be created using Logic Apps Designer

1. Auzre Logic Apps Canvas
2. Using JSON based templates

Azure Logic Apps make use of connectors - a logic app component that provides interface to external services. There are serveral connectors such as twitter connector and office 365 connector

### Azure Power Automate

Power automate allows you to create workflows using Power Automate website or app without any prior IT experience or knowledge.

There are four types of flows one can create

1. Automated - triggered on an event
2. Button - a repetative task to be triggered
3. Scheduled - one that occurs at a particular time
4. Business process - anything that replicates a business process (Eg. Ordering a stock)

Azure Power Automate uses Logic apps under the hood.

|                | Azure Power Automate               | Auzre logic Apps                    |
| -------------- | ---------------------------------- | ----------------------------------- |
| intended users | office Workers and business people | IT pros                             |
| Design Tools   | GUI Only. Web and Mobile           | Code editing is possible            |
| Usage          | Self service                       | Advanced integration                |
| App Life cycle | Test and production envt available | can be included in DEVOPS pipelines |

<br />
<br />

## Code-first Approach

This approach is preferred when the users want more control over the workflows
and their performance

### WebJobs and Webjob SDKs

Azure app service allows users to deploy web apps and Rest APIs. Azure WebJobs are part of Azure app service. They provide customization of JobHost, (eg. deciding number of retrys to external service)

1. Continous: That are running always (checking for photo in blob)
2. Triggered: based on an event

We can code the webjob in any of the high level programming language (node, python). If using any .NET based language we have SDKs that provide classes that help in creating webjobs

### Azure Functions

We can run the code using Auzre functions without thinking about Infrastructure on which it runs and pay for only the time during which the code runs. Can be created in any of the highlevel scripting languages. Automatically scaled based on demand.

You can start working on code in the Azure Portal itself. Can use git to control version. There are servral triggeres such as HTTP trigger, Blob trigger templates available to create Azure Functions

<br />

Comparision

We should use WebJobs instead of Functions in the below cases

- If the feature is part of existing app service, it can be handled using smae pipelines
- Close control over the object that triggers events, JOBHOST class from WebJOB SDK for .NET provides better control

|                           | Azure WebJobs                                         | Azure Functions |
| ------------------------- | ----------------------------------------------------- | --------------- |
| Languages supported       | All languages. SDK supports only .NET based languages | All languages   |
| Automated Scaling         | no                                                    | yes             |
| pay as you use            | You have to pay for the VM or app service plan        | yes             |
| Can be part of Appservice | Yes                                                   | No              |
| control over JobHost      | Yes                                                   | No              |
| Integrate with Logic apps | No                                                    | Yes             |
| Cam be tested in browsers | No                                                    | Yes             |

 <br />

---

## How to choose the approaches

![flowchart from Learning path](https://docs.microsoft.com/en-us/learn/modules/choose-azure-service-to-integrate-and-automate-business-processes/media/3-service-choice-flow-diagram.png)

Choose design first

- if the people are not developers
- want flow to be established

Choose code first

- if you want control over the workflow (you are a developer)
- hide unnecessary details to others

(refer flow chart)

mixing the technologies - you are not bound to use code first or design first approach. You can call one workflow from another. use a mix of the approaches to get a greated control over the smaller parts of the workflow..

## Choosing Design First approaches

(Consider bike rental system)
You want to automate the booking of the bike at a university and another university far off that already has bike rental system in place.

Understand the business process

![flowchart from Learning path](https://docs.microsoft.com/en-us/learn/modules/choose-azure-service-to-integrate-and-automate-business-processes/media/4-bike-hire-workflow.png)

It seems that it requires some customizaition with REST API. Hence use Azure Logic Apps

## Choosing Code First approaches

Consider you want to build a system that tracks the details of what repairs are done or due on bike before being rented out.

![bike repair approach](https://docs.microsoft.com/en-us/learn/modules/choose-azure-service-to-integrate-and-automate-business-processes/media/5-bike-maintenance-workflow.png)

This requires connection with DB and a task that can order parts from third party providers. With WebJobs you have to pay for the entire VM or appservice plan. Hence use Functions
