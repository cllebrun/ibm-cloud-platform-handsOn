# Introduction

# Getting started with Node.js and Cloudant on the IBM Cloud Platform
By following this guide, you'll set up a development environment, deploy an app locally and on the IBM Cloud Platform, and integrate a database service in your app.

<p align="center">
  <img src="https://raw.githubusercontent.com/IBM-Bluemix/get-started-java/master/docs/GettingStarted.gif" width="300">
</p>


# Objective

In the following lab, you will learn:

+ How to deploy a new Cloud Foundry app based on Node.js runtime
+ How to run a Node.js app locally
+ How to create a new database service  
+ How to use the Cloud Foundry Command Line Interface


# Pre-Requisites

+ Get an [IBM Cloud Platform account](https://console.bluemix.net/registration/), or use an existing account.
+ Install the [IBM Cloud CLI](http://clis.ng.bluemix.net/ui/home.html)
+ Install the [Git CLI](https://git-scm.com/downloads)
+ Install [Node.js](https://nodejs.org) to test locally


# Steps

1. [Clone a sample application](#step-1---clone-a-sample-application)
2. [Run the app locally](#step-2---run-the-app-locally)
3. [Prepare the app for deployment](#step-3---prepare-the-app-for-deployment)
4. [Deploy the app](#step-4---deploy-the-app)
5. [Add a database](#step-5---add-a-database)
6. [Use the database](#step-6---use-the-database)


# Step 1 - Clone a sample application

Now you're ready to start working with the simple Node.js *hello world* app. Clone the repository and change to the directory to where the sample app is located.
  ```
  git clone https://github.com/IBM-Bluemix/get-started-node
  ```

  ```
  cd get-started-node
  ```

# Step 2 - Run the app locally

Install the dependencies listed in the [package.json](https://docs.npmjs.com/files/package.json) file to run the app locally.  
  ```
  npm install
  ```

Run the app.
  ```
  npm start  
  ```

View your app at: http://localhost:3000

# Step 3 - Prepare the app for deployment

To deploy to the IBM Cloud Platform, it can be helpful to set up a manifest.yml file. One is provided for you with the sample. Take a moment to look at it.

The manifest.yml includes basic information about your app, such as the name, how much memory to allocate for each instance and the route. In this manifest.yml **random-route: true** generates a random route for your app to prevent your route from colliding with others.  Replace 'nodejs-helloworld' with the name of your choice for your application. You can also replace **random-route: true** with **host: myChosenHostName**, supplying a host name of your choice but it has to be unique. [Learn more...](https://console.bluemix.net/docs/manageapps/depapps.html#appmanifest)
 ```
 applications:
 - name: nodejs-helloworld
   random-route: true
   memory: 128M
 ```

# Step 4 - Deploy the app

You can use the the IBM Cloud Platform CLI to deploy apps.


1. Chose the *API-endpoint* in the command with an API endpoint from the following list.

  * US SOUTH: https://api.ng.bluemix.net
  * US EAST: https://api.us-east.bluemix.net
  * UK: https://api.eu-gb.bluemix.net
  * GERMANY: https://api.eu-de.bluemix.net
  * SYDNEY: https://api.au-syd.bluemix.net
   
  ```
   bx api <API-endpoint>
  ```

1. Login to your Bluemix account

  ```
  bx login
  ```
1. Target the right org and space on the IBM Cloud Platform

  ```
  $  bx target -o [your_org_name] -s [your_space_name]
  ```


1. From within the *nodejs-helloworld* directory push your app to Bluemix
  ```
  bx app push
  ```

This can take a minute. If there is an error in the deployment process you can use the command `bx cf logs <Your-App-Name> --recent` to troubleshoot.


1. View your app at the URL listed in the output of the push command, for example, *myUrl.mybluemix.net*.  You can issue the
```
bx apps
```
command to view your apps status and see the URL.


# Step 5 - Add a database

Next, we'll add a NoSQL database to this application and set up the application so that it can run locally and on the IBM Cloud Platform.
You can do it using the IBM Cloud UI or the CLI:

1. Using the CLI:

1. the Watson TTS service could have been provionned by running the command line, give it a name like "cloudantNoSQLDB-node":

```
  $ bx cf create-service cloudantNoSQLDB lite [your_service_name]

```

1. Bind the Watson service to your app:

```
  $ bx cf bind-service [your_app_name] [your_service_name]

```
1. Restage your app:

```
  $ bx cf restage [your_app_name]

```

1. Get your VCAP_SERVICES:

  Your application will restart and the service connection information will be made available to your application.

  ```
  bx cf env <your_app_name>

  ```
and you'll find the credentials under: 

"VCAP_SERVICES": {
  "cloudantNoSQLDB": [
   {
    "credentials": { ...


1: Using the UI:

1. Log in to the IBM Cloud Platform in your Browser. From the Catalog section, create the `Cloudant NoSQL DB` int the same region/org/space than you already have your app.
2. From your app overview, click 'Create Connection' under the 'Connections' section, chose the Cloudant instance you've just created.
3. Select `Restage` when prompted. Bluemix will restart your application and provide the database credentials to your application using the `VCAP_SERVICES` environment variable. This environment variable is only available to the application when it is running on the IBM Cloud Platform.

# Step 6 - Use the database

We're now going to update your local code to point to this database. We'll create a json file that will store the credentials for the services the application will use. This file will get used ONLY when the application is running locally. When running in the IBM Cloud Platform, the credentials will be read from the VCAP_SERVICES environment variable.

1. Create a file called `vcap-local.json` in your app directory with the following content:
  ```
  {
    "services": {
      "cloudantNoSQLDB": [
        {
          "credentials": {
            "url":"CLOUDANT_DATABASE_URL"
          },
          "label": "cloudantNoSQLDB"
        }
      ]
    }
  }
  ```

2. Back in the Bluthe IBM Cloud Platformemix UI, select your App -> Connections -> Cloudant -> View Credentials

3. Copy and paste just the `url` from the credentials to the `url` field of the `vcap-local.json` file.

4. Run your application locally.
  ```
  npm start  
  ```

  View your app at: http://localhost:3000. Any names you enter into the app will now get added to the database.


5. Re-deploy to the IBM Cloud Platform!
  ```
  bx app push
  ```

  From the the IBM Cloud Platform UI in your browser your can access your Database UI and check the names in your database:
  Select your App -> Connections -> Cloudant logo -> Launch -> mydb

  ![Todo](./images/cloudant.png)

  Look at the server.js file, this is where are implemented the get and post method to store and retrieve data in the cloudant database.

  # Resources

For additional resources pay close attention to the following:

- [Cloudant documentation](https://console.ng.bluemix.net/docs/services/Cloudant/index.html#getting-started-with-cloudant)

