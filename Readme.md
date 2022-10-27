dd## **Github Approval Based Pipeline**
new line
## Name

The name of your workflow that is displayed on the Github actions page. If you omit this field, it is set to the file name.

`````````````````````````````````````````````````` 
Name:  Approval based pipeline.
``````````````````````````````````````````````````

The on keyword defines the Github events that trigger the workflow. You can provide a single event, array or events or a configuration map that schedules a workflow.

``````````````````````````````````````````````````
on: push
# or
on: [pull_request, issues]
``````````````````````````````````````````````````

## Jobs

A workflow run is made up of one or more jobs. Jobs define the functionality that will be run in the workflow and run in parallel by default.

``````````````````````````````````````````````````
jobs:
  Build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Compile
        run: echo Hello, world!
``````````````````````````````````````````````````
## Runs-on

The runs-on keyword lets you define the OS (Operating System) your workflow should run on, for example, the latest version of ubuntu.

`````````````````````````````````````````````````
runs-on: ubuntu-latest
``````````````````````````````````````````````````

## Steps

A step is an individual task that can run commands in a job. A step can be either an action or a shell command. Each step in a job executes on the same runner, allowing the actions in that job to share data with each other.

``````````````````````````````````````````````````
steps:
      - uses: actions/checkout@v2

      - name: Compile
        run: echo Hello, world!
``````````````````````````````````````````````````

## Uses

The uses keyword tells the job to retrieve v2 of the community action named actions/checkout@v2. This is an action that checks out your repository and downloads it to the runner, allowing you to run actions against your code (such as testing tools). You must use the checkout action any time your workflow will run against the repository's code or you are using an action defined in the repository.  


``````````````````````````````````````````````````
uses: actions/checkout@v2
``````````````````````````````````````````````````
Environment-

Environment defines a map of environment variables that are available to all jobs and steps in the workflow. You can also set environment variables that are only available to a job or step.

needs:

This context is only populated for workflow runs that have dependent jobs, and changes for each job in a workflow run. You can access this context from any job or step in a workflow. This object contains all the properties listed below.

Creating dependent jobs

By default, the jobs in your workflow all run in parallel at the same time. So if you have a job that must only run after another job has completed, you can use the needs keyword to create this dependency. If one of the jobs fails, all dependent jobs are skipped; however, if you need the jobs to continue, you can define this using the if conditional statement.

In this example, the build job run in series, with deploy of code in  Dev environment when pull request is raised,post confirm merge request it will run in staging env and wait for approval post approval build will pass to production env and wait for approval and post approval pipeline will run in production env.

``````````````````````````````````````````````````
DeployDev:
    name: Deploy to Dev 
    if: github.event_name == 'pull_request'
    needs: [Build]
    runs-on: ubuntu-latest
    environment: 
      name: Development
      url: 'http://dev.myapp.com'
    steps:
      - name: Deploy
        run: echo I am deploying! 
    
  DeployStaging:
    name: Deploy to Staging 
    if: github.event.ref == 'refs/heads/main'
    needs: [Build]
    runs-on: ubuntu-latest
    environment: 
      name: Staging
      url: 'http://test.myapp.com'
    steps:
      - name: Deploy
        run: echo I am deploying! 
            
  DeployProd:
    name: Deploy to Production 
    needs: [DeployStaging]
    runs-on: ubuntu-latest
    environment: 
      name: Production
      url: 'http://www.myapp.com'
    steps:
      - name: Deploy
        run: echo I am deploying! 
``````````````````````````````````````````````````
## WorkFlow

![image Link](https://github.com/suyeshk7/github_approval/blob/main/images/Capture.PNG)
