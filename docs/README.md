# Hosting a Full-Stack Application

### **This is a project of implement a deployment process of a developed full stack application to a cloud service provider so that it is available to customers. This application contains the main components of a 3-tier full stack application (UI, API, and Database)..**

---
## project summary  
In this project i will explain the all steps needed to perform the deployment process of this full-stack app to a cloud service provider so that it is available to customers, using the aws console to start and configure the services the application needs such as a database to store product information and a web server allowing the site to be discovered by potential customers.

## steps of working on the project

1. Approach a good understand of the app:
    - Explore the package.json in order to understand the app details.
    - Research on the app's framework to understand at a high level the ideas behind the framework. 
    - Identify any communication to other applications in the code.
2. Identify the services needed by the app:
    - database
    - working backend server
    - working UI

## Project dependencies for development

```
- Node v14.15.1 (LTS) or more recent. While older versions can work it is advisable to keep node to latest LTS version

- npm 6.14.8 (LTS) or more recent, Yarn can work but was not tested for this project.

- AWS CLI v2.

- A RDS database running Postgres.

- A S3 bucket for hosting uploaded pictures.

```

## Configuring the project infrastructure supplying all required environment variables

### Configuring an AWS Relational Database Service (RDS)

- In the AWS RDS console i created a postgresql database with a public connectivity for the app to store data. 
    [RDS](database-1.crakqfejqysg.us-east-1.rds.amazonaws.com)
-  Test the app server connection to the DB instance to retrieve the data in the database.

### Configuring an AWS Elastic Beanstalk (EB)

- It is a free service that lets you easily run applications on web servers.
- This serves provides the Elastic Compute Cloud (EC2): Used for hosting servers, and Simple Storage Service (S3): Used for storing application code and sending it to other servers. 
- I created udagram-api EC2 with eb-cli with the environment udagram-api-env which is the back-end server to serve my udagram app, and upload the api src code required to operates the api in the S3 with the health ok.
[udagram-api](udagram-api-env.eba-phwjqsrw.us-east-1.elasticbeanstalk.com)

### AWS Simple Storage Service (S3)

- It is an AWS's file storage solution that drives many different connections between AWS services.
- I created and configured an S3 serves with aws s3 cli for Web hosting a static assets with enabling public reads, and upload the static website files to it with the working link:
[frontend-url](http://mygogobucket.s3-website-us-east-1.amazonaws.com)

## Run the app locally 
```
    - in the front-end directory 
    run the script:
    npm install
    npm build
    - fixing ayn founded errors 

    -- in the api directory 
    run the script:
    npm install
    npm build
    - fixing ayn founded errors 
```
## Deployment process

### deploy the app backend
```
- edit .elasticbeanstalk/config.yml as the following
    branch-defaults:
    artifact: www/Archive.zip
    default:
        environment: udagram-api-env
    deploy: null
    global:
    application_name: udagram-api
    branch: null
    default_ec2_keyname: null
    default_platform: Node.js 14 running on 64bit Amazon Linux 2
    default_region: us-east-1
    include_git_submodules: true
    instance_profile: null
    platform_name: null
    platform_version: null
    profile: default
    repository: null
    sc: null
    workspace_type: Application

- add environment variables

- in the project directory:

- eb init 
identify the region, app name, and node version
- eb create <environment-name>
- eb deploy
- after deploying the api 
    eb list // to view the available environments 
    eb use <the created environment>
    eb health //to view the env health 
    eb open // to open the env-url 
    eb --help // to get a needed command
```
## Deploy the front-end app
```
- in the app directory 

- in the src code we have to fined the connections to the api server and replace it with the url of the back-end api
- configure aws s3 service
    aws configure // then enter access key and access secret key
- create an aws s3 bucket 
    aws s3 mb s3://<bucket-name>
- upload the content of the build folder to the s3 bucket with a public access and identify a web hosting with index.html file through the aws s3 console or aws s3 cli
    aws s3 cp --recursive --acl public-read ./www s3://<bucket-name>/
- To ensure the browser take the new content of our HTML files, we need to force the browser to revalidate the content of the files in S3 with 
aws s3 cp --acl public-read --cache-control="max-age=0, no-cache, no-store, must-revalidate" ./www/index.html s3://<bucket-name>/
```
### After the manual deployment of all the app parts rds, UI, and api
    - we have to test the s3 link which represent the static website.
    - run the app and test the connection between all the app parts

## before creating a pipeline
### edit the '.circleci/config.yml' file with the necessary instructions:
- The orbs section which are responsible for setting up some basic recipes
- The jobs section which contain specific actions to take
- The workflows section which specify how the jobs should be handled   

## create a pipeline
1. In my github account i created an new repo with the name of the project
2. in the project directory use a 'git init' to initiate a locally git repo
3. I cloned the provided the local project repo to my new created github repo  
4. In the circleci platform i created an account with my github profile 
5. I select my project with the '.circleci/config.yml' with the main branch
6. add access key, access secret key, and region as an environment variables through the project settings pan the environment variables
7. return to my project repo and make any un influenced change in the project repo
8. use 'git status' to get the change 
9. use 'git add <file-that-have-changed>' to add the change to the repo
10. 'git commit -m "commit details" ' to describe the change
11. 'git push --force -u origin' to push the change to the repo
12. github repo change will trigger the circleci pipeline to follow the confirmed instructions in the .circleci/config.yml file, doing all steps automatically for you to deploy a new version of the app
13. the pipeline doing all steps as you did them manually for you and this is the useful of it  

## pipeline stages

### build

- Spin up environment

- Preparing environment variables

- Install Node.js 14.15

- Checkout code

- Install Front-End Dependencies

- Install API Dependencies

- Front-End Lint

- Front-End Build

- API Build
### hold require an approval 

### deploy

- Spin up environment

- Preparing environment variables

- Install Node.js 14.15

- Setting Up Elastic Beanstalk CLI

- Install AWS CLI - latest

- Configure AWS Access Key ID

- Checkout code

- Deploy App




