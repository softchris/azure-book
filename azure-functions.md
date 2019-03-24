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
 
 ## Verify your app
 check that it has a public URL and it is possible to navigate to its default page

 ![](/assets/Screen Shot 2019-03-24 at 01.53.19.png) 
 
## Triggers
Functions are event driven, which means they run in response to an event.
 
The type of event that starts the function is called a trigger. You must configure a function with exactly one trigger.

Azure supports the following:
- **Blob storage**,Start a function when a new or updated blob is detected.
- **Cosmos DB**, Start a function when inserts and updates are detected.
- **Event Grid**, Start a function when an event is received from Event Grid.
- **HTTP**, Start a function with an HTTP request.
- Microsoft Graph Events, Start a function in response to an incoming webhook from the Microsoft Graph. Each instance of this trigger can react to one Microsoft Graph resource type.
- **Queue storage**, Start a function when a new item is received on a queue. The queue message is provided as input to the function.
- **Service Bus**, Start a function in response to messages from a Service Bus queue.
- **Timer**, Start a function on a schedule.

### Bindings
- Bindings are a declarative way to connect data and services to your function
- Bindings know how to talk to different services
- Each binding has a direction - your code reads data from input bindings and writes data to output bindings
- Each function can have zero or more bindings to manage the input and output data processed by the function 
- A trigger is a special type of input binding that has the additional capability of initiating execution

> you don't have to write code in your function to connect to data sources and manage connections

Sample binding definition

```json
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

## Creating a Function, for your Function app
When adding your first function, you are presented with the Quickstart screen. This screen allows you to choose a trigger type (HTTP, Timer, or Data) and programming language (C#, JavaScript, F# or Java). Then, based on your selections, Azure will generate the function code and configuration for you with some sample code provided to display out the input data received in the log

Example of templates to choose from:

- HTTP trigger w/ C#, F#, or JavaScript
- Timer trigger w/ C#, F#, or JavaScript
- Queue trigger w/ C#, F#, or JavaScript
- Service Bus Queue trigger w/ C#, F#, or JavaScript
- Cosmos DB trigger w/ C# or JavaScript
- IoT Hub (Event Hub) w/ C#, F#, or JavaScript
  
Let's create that function:
![](/assets/Screen Shot 2019-03-24 at 02.05.11.png)

Now you are faced with the choice of how to author your Function:

- VS Code, this is a great choice, plenty of plugins supporting this option
- Any editor + Core tools, a more agnostic choice
- In-portal, you will write code in the Portal

![](/assets/Screen Shot 2019-03-24 at 02.06.17.png)
  
For now we will go with the `In-portal` option, we are now faced with:
- Webhook + API, function will run as soon as a certain URL is hit
- Timer, function will run according to a schedule
- More templates, there a ton more templates worth exploring
 
![](/assets/Screen Shot 2019-03-24 at 02.08.25.png)

For now we will go with the `Webhook + API` option.

This is now our coding environment:

![](/assets/Screen Shot 2019-03-24 at 02.10.47.png)

### Taking it for a spin
We can test it in the portal or hit the URL:

If we press the play button we get this:
![](/assets/Screen Shot 2019-03-24 at 02.13.06.png)
Ans on our right in the Portal we get this. As you can see we have specified a request body and we see that as well as what the function returns at the bottom of the below image: 
![](/assets/Screen Shot 2019-03-24 at 02.20.10.png)

Or we click `get function URL` and test it in a browser:
![](/assets/Screen Shot 2019-03-24 at 02.14.00.png)

After you've chosen to copy the URL you head to the browser input the URL and make sure to add &name=chris. Cause if you look at your Javascript code you see that it expects the parameter `name` in either the `body` or as a URL parameter. It should look like this:
![](/assets/Screen Shot 2019-03-24 at 02.17.32.png)

### Monitoring
We can also check out monitoring. It is able to provide us with all sorts of critical info such as:
history of function executions and displays the timestamp, result code, duration, and operation ID populated by Application Insights










