# Repository purpose

This reposistory is a work in progress. It will be noted when it is complete here. 

This is a demo app for Aklivity Zilla built on top of the example todo service from Aklivity to create a secondary chart-app that allows realtime rendering of poll data.

The todo app and todo service come directly from Aklivity's demo. You can find it [here](https://docs.aklivity.io/zilla/get-started/build-todo-app).

Comments are included throughout the Svelte 'chart-app' to help understand how Zilla helps make deploying applications using the power of Apache Kafka easier. 

Zilla lets you focus on what happens when data reaches point A or B not how you are going to get it between point A and B. Using Zilla for the first time is 
kind of like the first time you learned how to use pipes on a command line interface like BASH; you'll open up a whole new world of possibilites. 

Read a short blog post about the chart-app here: [pending] 

# Vercel Deployment

You can test the application yourself. Build and ensure the todo app from aklivity is running on your computer (make sure you do the app without authentication security added). 

Then you can visit this webpage [app](https://aklivity-chartdemo-deploy.vercel.app/). If all is done correctly, you should be able to fill out the poll page with responses and watch your chart populate live. 

Note: the deployment version of the Vercel app is stored in a separate repository to ensure Vercel can deploy error free. That repository is here: https://github.com/J-Rebs/aklivity-chartdemo-deploy. The code though between that repository and the chart app in this repository should be identical. 

