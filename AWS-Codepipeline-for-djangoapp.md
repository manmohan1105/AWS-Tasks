# Create a elastic bean app and enviroment  of python  and push your code to github create a AWS codepipeline for that app connect it to tha eb app so whenever you create any change in the github app that changes get automatically  deployed into your ec2 app

## Step1 : Create a eb app and python enviroment setup for your app.
          1. go to eb console then create a app
          2. create a env for your app from the same eb console
## Step2 : Create a pipeline for your app
          1. go to the awscodepipeline console create a pipeline from there
          2. choose a service role
          3. Source Provider Githubversion 1  -connect with your github account
          4. for change detection choose Github Webhooks
          5. Select your Repo and upload your code to github
          for django app you need to create a .ebextension directory and .django.config file to make elastic bin know which file to run how to start your app similary for other framework for  flask it is easy just apy file to it will run directory.
          example this app https://github.com/manmohan1105/awsdeploy 
