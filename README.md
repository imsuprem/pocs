Angular App with Cognito Authentication
===================================================

## What does this app do?
Connnects to a Cognito User pool and allows for Login / Register funtionality.

## Tech Stack
### Required Tools
* [aws cli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)
* [npm](https://www.npmjs.com/)
* [angular-cli](https://github.com/angular/angular-cli)

### Frameworks
* [AWS JavaScript SDK](http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/browser-intro.html)
* [Angular 2](https://angular.io/docs/ts/latest/quickstart.html)
* [TypeScript](https://www.typescriptlang.org/docs/tutorial.html)
* [Bootstrap](http://getbootstrap.com/)

## AWS Setup
##### Install the required tools (The automated installation script for AWS resources uses AWS CLI and runs on linux only.)
* Create an AWS account
* Install [npm](https://www.npmjs.com/)
* [Install or update your aws cli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) 
* Make sure your AWS CLI is configured by running aws configure. SDK uses these settings to connect to Cognito 
* [Install angular-cli](https://github.com/angular/angular-cli)


## Getting the code and running it locally
_This uses the pre-configured AWS resources hosted by AWS_

```
# Clone it or download the code

```
```
# Install the NPM packages
cd <your project directory>
npm install
```
```
# Run the app in dev mode
npm start
```

## Creating AWS Resources
This sample application can be deployed to S3. S3 will host this application as a static site.

* [What is S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)

**createResources.sh requires your [aws cli to be configured](http://docs.aws.amazon.com/cli/latest/userguide/controlling-output.html) for JSON output.**

```
# Install the AWS resources and deploy your application to S3
cd aws
./createResources.sh
```

Running the above command will create the necessary AWS resources and build & deploy your code to AWS. 
It will prompt you to choose your deployment target (S3 or Elastic Beanstalk). Choosing 'S3' makes your deployment
completely serverless, You must use S3 as the app is serverless 

*Caution:* You might incur AWS charges after running the setup script

## After initially running the ```createResources.sh``` script, use the below commands to rebuild and redeploy

### _S3:_ Update, Build and Deploy
```
# Build the project and sync the output with the S3 bucket
npm run build; cd dist; aws s3 sync . s3://[BUCKET_NAME]/
```
```
# Test your deployed application
curl –I http://[BUCKET_NAME].s3-website-[REGION].amazonaws.com/
```
__*NOTE: You might want to reshuffle some of the "package.json" dependencies and move the ones that belong to devDependencies 
for a leaner deployment bundle. At this point of time, AWS Beanstalk requires all of the dependencies, 
including the devDependencies to be under the dependencies section. But if you're not using Beanstalk then you can
optimize as you wish.*__

*or*

### _Beanstalk:_ Update, Build and Deploy
```
# Commit your changes in order to deploy it to your environment
git add .
git commit
eb deploy
```
```
# View your deployed application in a browser
eb open
```

## Local Testing

This section contains instructions on how to test the application locally (using mocked services instead of the real AWS services).

### LocalStack

To test this application using [LocalStack](https://github.com/localstack/localstack), you can use the `awslocal` CLI (https://github.com/localstack/awscli-local).
```
pip install awscli-local
```
Simply parameterize the `./createResources.sh` installation script with `aws_cmd=awslocal`:
```
cd aws; aws_cmd=awslocal ./createResources.sh
```
Once the code is deployed to the local S3 server, the application is accessible via http://localhost:4572/cognitosample-localapp/index.html (Assuming "localapp" has been chosen as resource name in the previous step)
