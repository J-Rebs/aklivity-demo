# About this Coffee Appreciation Poll demo for Aklivity ZIlla

If you have worked on a UNIX based system before, you can probably remember the first time you learned to use piping to redirect inputs and outputs between files and programs. It enabled modular programs that focused on small tasks which could then be chained together to complete more complex tests. Piping within a command line for an operating system saves time and enforces separation of concerns (i.e., modularity) in developing a software system. Aklivity Zilla brings the same idea of piping to the age of event streaming and real time updates.

Suppose you wanted to make a Coffee Appreciation Poll application as a website where a user could use sliders to give feedback on the reasons they appreciate coffee on one page. On a separate page, user feedback would populate in real time allowing the poll maker to get an up-to-date understanding of user sentiments. How would you build it? 

Well, you might want to use server-side events to ensure real time updates to the poll chart, and you would need something like an Apache Kafka cluster running in the background to log user feedback. You also need a way to send information to the cluster, maybe something like a POST method if you are building a web application. Without Zilla, you would need to separately build integrations from the server-side events and the REST API back into your Kafka cluster.

Zilla removes this concern. Using a simple JSON, you can stack different bindings with everything from http and related file system servers to server-side-event servers and Kafka clusters. All you have to do is specify when to enter and exit each type of server. In short, you need to pipe your data streams. This lets you focus on how data should be manipulated rather than worrying about how to move data. In the same way as piping and modular programming improved working on desktop operating systems, Zilla improves programming in an era of real time data streams. 


# Try out the application

You can test the application yourself. Build and ensure the todo app from Aklivity is running on your computer (make sure you do the app without authentication security added). You can find that demo guide [here](https://docs.aklivity.io/zilla/get-started/build-todo-app). 

Then you can visit this web [app](https://aklivity-chartdemo-deploy.vercel.app/). If all is done correctly, you should be able to fill out the poll page with responses and watch your chart populate live. 

# Other notes

The deployment version of the web app is stored in a separate repository to ensure Vercel can deploy error free. That repository is here: https://github.com/J-Rebs/aklivity-chartdemo-deploy. The code though between that repository and the chart app in this repository should be identical. 

This application is for demonstration purposes of Zilla's capabilities only.
