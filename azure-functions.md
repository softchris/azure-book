#Azure functions

- Decide if serverless computing is right for your business need.
- Create an Azure function app in the Azure portal.
- Execute a function using triggers.
- Monitor and test your Azure function from the Azure portal.

Serverless compute can be thought of as a function as a service (FaaS), or a microservice that is hosted on a cloud platform. 
- Your business logic runs as functions 
- you don't have to manually provision or scale infrastructure. 
- The cloud provider manages infrastructure. 
- Your app is automatically scaled out or down depending on load. 

Approaches for this kind of architecture
- Azure Logic Apps 
- Azure Functions

> Azure Functions is a serverless application platform. Developers can host business logic that can be executed without provisioning infrastructure

Currently supports C#, F#, and JavaScript

## Scales and billed only when processing work
Serverless computing helps solve the allocation problem by scaling up or down automatically, and you're only billed when your function is processing work

## Stateless logic
Stateless functions are great candidates for serverless compute; function instances are created and destroyed on demand. If state is required, it can be stored in an associated storage service.

## Event driven
Functions are event driven. 

> This means they run only in response to an event (called a "trigger"), such as receiving an HTTP request, or a message being added to a queue. 

You configure a trigger as part of the function definition. This approach simplifies your code by allowing you to declare where the data comes from (trigger/input binding) and where it goes (output binding). 

> You don't need to write code to watch queues, blobs, hubs, etc. You can focus purely on the business logic.

## Drawbacks

- **Execution time**, timeout of 5 minutes, This timeout is configurable to a maximum of 10 minutes. If your function requires more than 10 minutes to execute, you can host it on a VM. Additionally, if your service is initiated through an HTTP request and you expect that value as an HTTP response, the timeout is further restricted to 2.5 minutes, BUT there's also an option called Durable Functions that allows you to orchestrate the executions of multiple functions without any timeout
- **Execution frequency**, If you expect your function to be executed continuously by multiple clients, it would be prudent to estimate the usage and calculate the cost of using functions accordingly. It might be cheaper to host your service on a VM

## What is a function app?
`Functions` are hosted in an execution context called a `function app`

## Pre requisites
- **service plan**, *Consumption service plan* and *Azure App Service plan* CSP has automatic scaling and bills you when your functions are running and configurable timeout period for the execution of a function. By default, it is 5 minute, ASP allows you to avoid timeout periods by having your function run continuously on a VM that you define
- **storage account**, function app must be linked to a storage account. Uses this for internal operations such as logging function executions and managing execution triggers. Also function code and configuration file are stored here

## Creating a function app
In portal
- Create a resource / Compute / Function app
App name must be globally unique as it will serve as part of base URL
 - select a subscription
 - select existing resource group
 - select Windows for OS
 - Hosting plan should be Consumption plan
 - Select geography
 - Runtime stack, Javascript
 - Create a new storage account
 - Enable application insights
 CREATE





