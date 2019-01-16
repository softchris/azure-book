## Custom Vision Service Tutorial
The idea with using Custom Service is that you have a pre trained model already. However you may need the model to perform better in certain areas so this is where you add a specific type of images in a specific type of category to get it to recognize what you need.

Let's have a look at the below tutorial in which we learn to identify paintings from famous artists.

We will take the following steps:

- **Create** a Custom Vision Service project
- **Train** a Custom Vision Service model with tagged images
- **Test** a Custom Vision Service model
- **Call** your custom model's prediction endpoint over HTTP

## Create a Custom Vision Project

We need to go to the following URL https://www.customvision.ai/
Sign in and click `New Project`

You will be presented with the following modal:
![](/assets/Screen Shot 2019-01-16 at 16.47.25.png)

name the project `Artworks`, and make sure that `General` is selected in the `Domains` list. You can keep the `default settings` for `Project Types` and `Classification Types`. 

Select `Create project` to create our project.

## Upload tagged images
Click the `+` to the right of `Tags`:
![](/assets/Screen Shot 2019-01-16 at 16.50.40.png) 
Enter the title `painting`, like so:
![](/assets/Screen Shot 2019-01-16 at 16.51.30.png)

Repeat the steps to add `Picasso`, `Pollock`, and `Rembrandt`

So we now have this:
![](/assets/Screen Shot 2019-01-16 at 16.53.13.png)


