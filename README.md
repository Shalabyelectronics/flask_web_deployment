# Flask web deployment 

In this repository we will going to deploy our flask web app with Heroku, by explaining step by step you need to take to be able to successfully deploy  your web app on the Heroku server and let all your friends and family to see your creativities , so Lets Fun begin.

Overview the end goal from our lecture 

![overview](https://user-images.githubusercontent.com/57592040/171042191-d2697434-c6d8-4f20-b1ac-63747b793738.gif)


[Here is Shalaby's Blog](https://shalaby-blog.herokuapp.com/)

## STEP 1:

We need a web app to deploy first so if you don't have one yet you can practice with this web app we have here just download the repository edit it then share it to your GitHub account.

So now you have your web app and ready to go to the next step >>

## Step 2:

Create a new account with Heroku ? But first why Heroku and what is the main service we need to get from them ?? 

Heroku is a platform as a service (PaaS) that enables developers to build , run, and operate application entirely in the cloud and in easy English you can be able to shift your app from a development mode to a production mode.

So after you created your Heroku account you are ready to deploy your web app so lets jump to the next step>>

## Step 3:

You need to notice that shifting your web app from your machine to Heroku machine (It is a server but just for simplify it) ,it need some configuration to add but **first** before doing this movement all that we need now is to connect our remote repository from GitHub to Heroku and it is very easy step as show below:

![connent your github with heroku](https://user-images.githubusercontent.com/57592040/171042268-972606fc-9663-4817-a236-920c4442e838.gif)


after your web app successfully deployed and when pressing the view button you will see this page??

![application error](https://user-images.githubusercontent.com/57592040/171042312-b65e1046-bba7-49bb-8f12-d408e4b73081.png)


That mean there is something missing here but what is that?? lets see on next step >>

## Step 4:

Ok from the previous step we did good job so far but we can't view our web app so why is that? lets recall this message when you running your web app in development mode:

![production WSGI](https://user-images.githubusercontent.com/57592040/171042381-5209ac84-b4de-443e-8bab-a60faa901d14.png)

yes this is the message with yellow highlight and here another copy for it 

```bash
WARNING: This is a development server. Do not use it in a production deployment.
Use a production WSGI server instead.
```

So here we know that development server is our localhost server that flask created for us but what is `WSGI` server?? 

WSGI is a Web Server Gateway Interface for more details check [here](https://www.python.org/dev/peps/pep-3333/)

And in simple way when the web app running in development server (Our computer) it's know how to communicate with each others I mean between the web app and the server but in Web server it doesn't know how to do this communication  so we need to use (WSGI) to do that. (To be honest I didn't get the full image about what is going on but just keep going)

So we need to install `gunicorn` WSGI server because it is the most popular one. if you are using pycharm you can go to setting then project interpreter as below:

![install gunicorn](https://user-images.githubusercontent.com/57592040/171042437-032b848e-9824-4641-9a80-212f8fb6847e.png)


After you installed it you need to add it to requirements file as below:

![add gunicorn to requirments](https://user-images.githubusercontent.com/57592040/171042456-62dde42e-a8dd-4b05-b1b7-b99eefcbb4cf.png)


finally we need to create a file that will run the WSGI and this file called `Procfile` without any extension and you will add just one line of code as below:

This line will be read it by Heroku server to run the WSGI server that will know how to connect our web app to the web server. 

And when you try out congratulations your web app is running but we didn't done yet .

## Step  5:

Now we want to upgrade our SQLite Database to PostgreSQL but why?

Regarding [Heroku](https://devcenter.heroku.com/articles/sqlite3) that because SQLite is good for a development because SQLite runs in memory, and backs up its data store in files on disk. and to understand more about what is a system that using within Heroku server and what is their file systems you can read about it [here](https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem).

So to convert our SQLite to PostgreSQL fellow the constructers below :

![add postgresql](https://user-images.githubusercontent.com/57592040/171042508-fa9ed22e-bd60-4faa-907b-2d2a0d5d8959.gif)


Then we need to add the DATABASE URI as environment variable and the secret key as will so here how we do that:

![add enviroment variable](https://user-images.githubusercontent.com/57592040/171042529-2ae68418-e503-4ab4-bd90-ae31a26d8b81.gif)


Then you need to change the configuration to your web app as shown here:

![change configration to get it from env](https://user-images.githubusercontent.com/57592040/171042564-783deca9-eda7-47f0-aab1-1cc4e8eb9e5e.png)


Finally we need to install [psycopg2-binary](https://pypi.org/project/psycopg2-binary/) packages that allow PostgrSQL to work well Also we need to add it to our requirements text file with the current version as below:

![install psycopg2-binary](https://user-images.githubusercontent.com/57592040/171042596-de4f38e3-2bad-44f1-a229-b44270104643.png)


and before you push your commits to GitHub remote repository lets add `.gitignore` file and we can pick what files will be ignore when pushing our repository by this wonderful service here and fellow the step as below to get the content for your flask web app:

![git ignore](https://user-images.githubusercontent.com/57592040/171042615-2f355d7a-507c-4e6e-a555-f3861b3757e8.gif)


That's it now when you push your comments Heroku will detect the changes and rebuild our web app and here we are>>>> you can check it [here](https://shalaby-blog.herokuapp.com/)  

## Challenges 

I spent a lot of time trying to deploy my web app to Heroku, but I was always getting error http 500 that mean that my web app have a problem, one of very strange error it was mention in the log report that my BlogPost table is not define after 3 days I figured out that the error was with the body Column that Text type don't have length and because of that it could not defined the whole table.
so I jsut changed Text(250) to just Text as below:
```py
body = db.Column(db.Text, nullable=False)
```

## Resources

This Practice is part of [100 days of code: The complete python](https://www.udemy.com/course/100-days-of-code/) day 70

