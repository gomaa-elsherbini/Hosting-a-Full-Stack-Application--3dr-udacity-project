# Configuring the project infrastructure supplying all required environment variables

## Configuring an AWS Relational Database Service (RDS)

- In the AWS RDS console i created a postgresql database with a public connectivity for the app to store data. 
    [RDS](database-1.crakqfejqysg.us-east-1.rds.amazonaws.com)

### steps to configure aws rds
- [Creating a PostgreSQL DB instance and connecting to a database on a PostgreSQL DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.PostgreSQL.html)
   
-  [Test the app server connection to the DB instance to retrieve the data in the database with postpird tool](https://www.electronjs.org/apps/postbird).

### Configuring an AWS Elastic Beanstalk (EB)

- It is a free service that lets you easily run applications on web servers.
- This serves provides the Elastic Compute Cloud (EC2): Used for hosting servers, and Simple Storage Service (S3): Used for storing application code and sending it to other servers. 
- I created udagram-api EC2 with eb-cli with the environment udagram-api-env which is the back-end server to serve my udagram app, and upload the api src code required to operates the api in the S3 with the health ok.
[udagram-api](udagram-api-env.eba-phwjqsrw.us-east-1.elasticbeanstalk.com)

- [Create an Elastic Beanstalk environment](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_nodejs_express.html)


### AWS Simple Storage Service (S3)

- It is an AWS's file storage solution that drives many different connections between AWS services.
- I created and configured an S3 serves with aws s3 cli for Web hosting a static assets with enabling public reads, and upload the static website files to it with the working link:
[frontend-url](http://mygogobucket.s3-website-us-east-1.amazonaws.com)


- [Enabling Website Hosting on S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EnableWebsiteHosting.html)
- [Setting public access for website hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html)
- [How to create an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)

