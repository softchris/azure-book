# Serverless - from the beginning, using Azure functions ( Azure portal ), part I
![](/assets/dawn-dramatic-dusk-1102915.jpg)

Follow me on [Twitter](https://twitter.com/chris_noring), happy to take your suggestions on topics or improvements /Chris

> Serverless compute can be thought of as a function as a service (FaaS), or a microservice that is hosted on a cloud platform

This is the first part of three part series:
- Serverless - from the beginning, using Azure functions ( Azure portal ), part I, **you are here**
- Serverless - from the beginning, using Azure functions ( VS Code ), part II, in progress
- Serverless - from the beginning, using Azure functions ( Azure CLI ), part III, in progress


In this article we will cover the following:
- **Serverless**, What is Serverless and why might I need it
- **Function apps**, trigger and bindings
- **Functions** in function apps, languages it supports, authoring choices, testing it out, monitoring logging and setting of authorization level, working with parameters

In this article we already assume that putting your apps in the Cloud is a given. You've seen the benefits that means in terms of not having to maintain hardware, only pay for what you actually use and so on. 

## The many choices in the Cloud
Now, being in the Cloud means you have options, A LOT of options in fact. You can be on the lowest level deciding exactly what memory or hard drive type your apps can run on. Then you can be on a more managed level where you are happy to create a Virtual Machine, a so called VM, where you can install the OS and software you need. There are still more steps on this ladder namely running your applications in App Services where you don't have a VM anymore, just a place for your code to reside and yes you can decide what OS to run this on but that's pretty much it, it's a SAAS, software as a Service platform.
BUT, there is a step above that `Serverless`.  

## Introducing Serverless
So what does Serverless mean?
Serverless is essentially a function as a service `Faas`, or a microservice that is hosted on a cloud platform. 

Ok, what benefits does it offer then?
- **Everything is functions**, Your business logic runs as functions 
- **NO Manual provisioning*8, you don't have to manually provision or scale infrastructure. 
- **Managed infrastructure**, The cloud provider manages infrastructure. 
- **Automatic scaling**, Your app is automatically scaled out or down depending on load. 

## Serverless on Azure
Azure has two kinds of approaches for Serverless architecture 
- [Azure Logic Apps, intro](https://azure.microsoft.com/en-gb/services/logic-apps/?wt.mc_id=devto-blog-chnoring) Azure Logic Apps enables you to create powerful workflows 
- [Azure Logic Apps, documentation](https://docs.microsoft.com/en-gb/azure/logic-apps/?wt.mc_id=devto-blog-chnoring) 
- [Azure Functions](https://azure.microsoft.com/en-gb/services/functions/?wt.mc_id=devto-blog-chnoring), Azure Functions is a serverless application platform. Developers can host business logic that can be executed without provisioning infrastructure
- [Axure functions docs overview](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview?wt.mc_id=devto-blog-chnoring)

### What else is there to know about Serverless?
Is it all unicorns and rainbows?
Well, Serverless is definitely great but there are some things we need to know about them like:
- **They are stateless**, function instances are created and destroyed on demand. If state is required, it can be stored in an associated storage service
- **They are event driven**, they run only in response to an event (called a "trigger"), such as receiving an HTTP request, or a message being added to a queue. So essentially you declare where data comes from and where it goes. You do this declaratively with something called bindings which means you don't need to code to talk to queues, blobs, hubs, only business logic is needed
- **They do have Drawbacks**, drawbacks are in the form of limitations on *execution time* and *execution frequency*. 
 - **timeout**, The timeout is 5 minutes, This timeout is configurable to a maximum of 10 minutes. If your function requires more than 10 minutes to execute, you can host it on a VM. Additionally, if your service is initiated through an HTTP request and you expect that value as an HTTP response, the timeout is further restricted to 2.5 minutes, BUT there's also an option called [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?wt.mc_id=devto-blog-chnoring) that allows you to orchestrate the executions of multiple functions without any timeout
 - **Execution frequency**, If you expect your function to be executed continuously by multiple clients, it would be prudent to estimate the usage and calculate the cost of using functions accordingly. It might be cheaper to host your service on a VM

### Serverless vs App Services
It's easy to think that your first go-to is AppService that fits most likely with your mental model as a developers, you want to move the app that you have from On Premise to the Cloud and to do so you need to provision databases, creating your Services in App Service and that's it right? Well, most applications are seldom that simple, they tend to need to talk to a number of sub systems to maybe log in, or grab a piece of data somewhere or perform a computation. 

All these side things are maybe the concern of more than one app in your echo system so it makes sense to move them out into separate services. Then you might realise you only need to call on this services very seldom like when a new user is created or there is an incoming request. Your response at that point is maybe to place that incoming message on a Queue, or insert a row in a Database or maybe create a Slack notification. 

What we are saying here is that maybe we don't need to pay for a full AppService and the uptime and responsiveness it gives us, but instead we need a framework that can trigger a function based on a pre defined event and that can then carry out a computation that results in a side effect like calling another service/database/queue/whatever. 

Now we have come to the sweet spot where Serverless really shines, *seldom called services* that needs to do something in response to some kind of `event` happening.

In a word

> Serverless computing helps solve the allocation problem by scaling up or down automatically, and you're only billed when your function is processing work

## What is a function app?
`Functions` are hosted in an execution context called a `function app`. Which means what? Think of the Function app as the project you host your functions in. 

Currently you can use the following languages working with Function apps, C#, F#, and JavaScript

## Pre requisites
Ok, there are some things that needs to exist before we can get our function up there in the cloud. Those are:

- **service plan**, There are two choices of plans *Consumption service plan*, CSP and *Azure App Service plan*, ASP CSP has automatic scaling and bills you when your functions are running and configurable timeout period for the execution of a function. By default, it is 5 minute, ASP allows you to avoid timeout periods by having your function run continuously on a VM that you define
- **storage account**, function app must be linked to a storage account. It uses this for internal operations such as logging function executions and managing execution triggers. Also function code and configuration file are stored here

## Creating a function app
Now there are different ways of doing this we can create functions in the following ways:
- **Portal**, Using the Azure Portal
- **CLI**, Using the Azure CLI
- **VS Code**, Using VS Code to scaffold an Azure Function app and Azure Functions using some amazing plugins made for the purpose.

In this article we will focus on the first option but in doing so we will put some focus on some great concepts you need to know about, so it may be the least appealing option as a developer but the information the portal shows is great to know about, so stay with me cause we are about to do some coding next:

![](/assets/excited.gif)
  
### Select the correct template, Function App
Let's head to the portal and log in at 

> portal.azure.com

Once logged select the following :
![](/assets/Screen Shot 2019-03-25 at 20.36.43.png)

### Make the choices in the template
Once we've select the `Function app` template we need to select a few things. Your UI will at this point look something like this:
![](/assets/Screen Shot 2019-03-25 at 20.47.17.png)

Keep the following in mind though:

>App name must be globally unique as it will serve as part of base URL



Ok so the following choices will need to be made:
 - **Select a subscription**, well pick out one of the ones you have
 - **Select a resource group**, you can choose an existing resource group or create a new one, up to you 
 - **Select an OS**, choices here are Windows or Linux, we opt for Windows cause we need to select something :)
 - **Select Hosting plan**, this should be *Consumption plan*, we mentioned Consumption plans earlier in this article and why it's the better choice
 - **Select geography**, well select the region closest to you
 - **Runtime stack**, this is the language you are going to be coding in so we select Javascript
 - **Create a new storage account**, let's take an existing or create new one
 - **Enable application insights**, for stats and other types of application tracking
 
Now hit the button  **CREATE**

This takes a while, like a few minutes. Have some coffee or other hot beverage at this point:
![](/assets/hotbeverage.gif)
 
 ## Verify your app
 check that it has a public URL and it is possible to navigate to its default page

 ![](/assets/Screen Shot 2019-03-24 at 01.53.19.png)
 
Ok great we have a default page, now what? Well it's time to add a function to our Function app.
 
## Creating a Function, for your Function app
When adding your first function, you are presented with the Quickstart screen. 

Click the `New function` button and now you are face with this screen:



This screen allows you to choose a trigger type (HTTP, Timer, or Data) and programming language (C#, JavaScript, F# or Java). Then, based on your selections, Azure will generate the function code and configuration for you with some sample code provided to display out the input data received in the log

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
history of function executions and displays the timestamp, result code, duration, and operation ID populated by Application Insights. To see the Monitoring view we simply click here:
![](/assets/Screen Shot 2019-03-26 at 01.58.48.png)
On the right we are being presented with a view showing when the function was run, success/fail, and the duration it took to run the function

### Streaming log window
You're also able to add logging statements to your function for debugging in the Azure portal. The called methods for each language are passed a "logging" object, which may be used to log information to the log window.
To use it we call:
```
context.log('logging...');
```

This will be picked up by the `Logs` tab. `Logs` tab can be found at the bottom of the screen and can look like this:
![](/assets/logs.png)
Our log statement is highlighted

### Authorization
Going to the portal we can control how our function get's called but also who has the right to call it. To manage authorization we navigate like so:

![](/assets/Screen Shot 2019-03-26 at 02.09.23.png)

As you can see the field `Authorization level` allows us to choose `Authorization`. Available options are:
- Function, means a function specific API key is required
- Anonymous, no API key is required
- Admin, we need something called a *host key* to make it possible to call this function

By clicking the `Manage` menu option you are able to get something looking like this which will allow you to manage both your `host` and `function` keys:
![](/assets/Screen Shot 2019-03-24 at 22.13.58.png)

> So think about what authorization level is right for you, or in the immortal words of Gandalf
![](/assets/youshallnotpass.gif)
At least if they are a Balrog trying to use your Azure Function ;)

### Working with parameters
When you create new function in your Function app this is the default code you are left with. It tells us how to deal with parameters in both request URL and the BODY:
![](/assets/Screen Shot 2019-03-26 at 02.15.48.png)

Looking at the above code you see that it's reading query parameters with `req.query.[name of parameter]` and doing the same for the body with `req.body.[name of parameter]`.

## Build our service
Ok then shall we build something more fun than a `hello world`? Ok, it's not going to be ton more fun but it's at least something you can cringe over like a colleague of mine did, look Adam you've made it into the blog post haha.

Ok, we are going to build a... wait for it, drumrolls, a *dad jokes* service. You know those really bad puns told by a parent that compels you to deny any relations to them, yes those jokes. Google is my friend, so let's start Googling for some jokes and store them in a list:

```js
var dadJokes = [
  "Did you hear about the restaurant on the moon? Great food, no atmosphere.",
  "What do you call a fake noodle? An Impasta.",
  "How many apples grow on a tree? All of them.",
  "Want to hear a joke about paper? Nevermind it's tearable.",
  "I just watched a program about beavers. It was the best dam program I've ever seen.",
  "Why did the coffee file a police report? It got mugged.",
  "How does a penguin build it's house? Igloos it together."
];
```
There, if I can't unsee it, neither can you ;)

Ok next step is to find some pictures of dogs looking like they are laughing, because Internet right ;)

```js
var punDogs = [
 "image1.jpg",
 "image2.jpg"      
]
```
What, I didn't give you the actual image url, you're a big developer, I'm sure you can find pictures of dogs on the Internet, or why not cats ;)

Now to select a random dad joke and random image:

```js
var newNumber = Math.floor(Math.random() * dadJokes.length);
    var dogImageUrl = Math.floor(Math.random() * punDogs.length);
```

Lastly, let's answer with a HTML response:
```js
context.res = {
      status: 200,
      headers: {
        "Content-Type": "text/html"
      },
      body: '<h3>'+dadJokes[newNumber]+'</h3>' + '<br><img src="'+ punDogs[dogImageUrl] +'"/>'
    };
```

Let's save our code and make sure to set authorization level to Anonymous because why would we want to restrict the usage of this profound function to the outside world? ;)


## Summary
There is so much more to say on the Serverless topics. So many different ways we can trigger this function other than HTTP and integrations that are already there and waiting for you to use them. But we have to save something for future parts of this series. Hopefully you've gotten a good idea of what Serverless is and when to use it and how to create your first one of many Azure functions. See you in the next part hopefully :)

 









