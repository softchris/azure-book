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


