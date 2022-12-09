### After the manual deployment of all the app parts rds, UI, and api
    - we have to test the website link which represented with the s3 bucket link.
    - run the app and test the connection between all the app parts

## before creating a pipeline
### edit the '.circleci/config.yml' file with the necessary instructions:
- The orbs section which are responsible for setting up some basic recipes
- The jobs section which contain specific actions to take
- The workflows section which specify how the jobs should be handled   
'.circleci/config.yml'

'''
version: 2.1
orbs:
  # orgs contain basic recipes and reproducible actions (install node, aws, etc.)
  node: circleci/node@5.0.2
  eb: circleci/aws-elastic-beanstalk@2.0.1
  aws-cli: circleci/aws-cli@3.1.1
  # different jobs are called later in the workflows sections
jobs:
  build:
    docker:
      # the base image can run most needed actions with orbs
      - image: "cimg/node:14.15"
    steps:
      # install node and checkout code
      - node/install:
          node-version: '14.15'         
      - checkout
      # Use root level package.json to install dependencies in the frontend app
      - run:
          name: Install Front-End Dependencies
          command: |
            echo "NODE --version" 
            echo $(node --version)
            echo "NPM --version" 
            echo $(npm --version)
            npm run frontend:install
      # TODO: Install dependencies in the backend API          
      - run:
          name: Install API Dependencies
          command: |
            echo "TODO: Install dependencies in the backend API  "
            npm run api:install
      # TODO: Lint the frontend
      - run:
          name: Front-End Lint
          command: |
            echo "TODO: Lint the frontend"
            npm run frontend:lint
      # TODO: Build the frontend app
      - run:
          name: Front-End Build
          command: |
            echo "TODO: Build the frontend app"
            npm run frontend:build
      # TODO: Build the backend API      
      - run:
          name: API Build
          command: |
            echo "TODO: Build the backend API"
            npm run api:build
  # deploy step will run only after manual approval
  deploy:
    docker:
      - image: "cimg/base:stable"
      # more setup needed for aws, node, elastic beanstalk
    steps:
      - node/install:
          node-version: '14.15' 
      - eb/setup
      - aws-cli/setup
      - checkout
      - run:
          name: Deploy App
          # TODO: Install, build, deploy in both apps
          command: |
            echo "# TODO: Install, build, deploy in both apps"
            npm run deploy
            
workflows:
  udagram:
    jobs:
      - build
      - hold:
          filters:
            branches:
              only:
                - main
          type: approval
          requires:
            - build
      - deploy:
          requires:
            - hold
'''

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