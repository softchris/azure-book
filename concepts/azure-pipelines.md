# Azure pipelines

For CI/CD. More to come

Cresting your first pipeline

## Get sample code

```
git clone https://github.com/MicrosoftDocs/pipelines-javascript
```


## Get your first build
1. Sign in to your Azure DevOps organization and navigate to your project.
2. In your project, navigate to the Pipelines page. Then choose **New**, **New build pipeline**. 


Before doing all this fork the repository at:

> https://github.com/MicrosoftDocs/pipelines-javascript


Sign in by going to: 

> https://dev.azure.com

Click your project
![](/assets/Screen Shot 2019-03-08 at 17.20.29.png)
Click **New pipeline** button
![](/assets/Screen Shot 2019-03-08 at 17.23.12.png)
Select Github
![](/assets/pipeline-github.png)
You might be redirected to GitHub to sign in. If so, enter your GitHub credentials
You will also be prompted to Authorize
![](/assets/Screen Shot 2019-03-08 at 17.27.10.png)

When the list of repositories appears, select the your desired sample app repository
It should look something like this:
![](/assets/Screen Shot 2019-03-08 at 17.34.43.png)

It will ask you to authorize a bunch of times more and also ask to install Azure pipelines on all your repos or the selected one. 
![](/assets/Screen Shot 2019-03-08 at 18.21.26.png)

Then it will ask you for a template
![](/assets/Screen Shot 2019-03-08 at 18.24.05.png)
Go ahead and select the recommended one
![](/assets/pipeline-template.png)

Then it's showing you the `azure-pipelines.yaml`
![](/assets/Screen Shot 2019-03-08 at 18.26.56.png)

Press save and run
![](/assets/Screen Shot 2019-03-08 at 18.28.08.png)

It will ask to commit the `azure-pipelines.yaml` to master. Take **save and run** again

And we get this:
![](/assets/pipeline-build-failure.png)

What did we do wrong, well click `npm install and build` to get the why:
![](/assets/Screen Shot 2019-03-08 at 18.36.52.png)
AHA, it's telling us we are running `npm build` and that we are missing it. Let's verify that with our actual `package.json` file:
![](/assets/Screen Shot 2019-03-08 at 18.38.19.png)

We have a `build` action but it does nothing.. So let's go to our `azure-pipelines.yaml` and remove that step, cause we don't seem to need. If we do need it in the future we can always readd it.

That means we remove that from the `scripts` section of the file so:

```
- script: |
    npm install
    npm run build
```

becomes

```
- script: |
   
    npm install
```
Then we need to commit and push this change. After that we need to kick off another build to see if we solved it:
![](/assets/Screen Shot 2019-03-08 at 18.41.37.png)

We wait for the build to finish and we get:
![](/assets/Screen Shot 2019-03-08 at 18.49.39.png)

All green, and we did it!!!

## Adding tests
As we all know, a pipeline is only any good if it tells us wether we are delivering quality software. A way to know that is to add tests to your solution. What should happen IMO is:

- if tests fail, the pipeline should fail and no artifact should be created
- if tests succeed then pipeline should succeed and artifact should be created

## Deploy artificact to AppService ?

## Adding a badge



