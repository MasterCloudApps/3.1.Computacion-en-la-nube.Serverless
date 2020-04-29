# 2019-ServerlessVsExpress [![GitHub license](https://img.shields.io/github/license/codeurjc-students/2019-ServerlessVsExpress)](https://github.com/codeurjc-students/2019-ServerlessVsExpress/blob/master/LICENSE)
Comparative between serverless stacks (AWS Lambda) and Express in Node.js to create full fledged CRUD Web applications

# Development process

### The development process will be divided in the following sections

- **Step 0. Configuration of Visual Studio Code to use it with Node.js and AWS SAM**
    * In this section, we will see how to **configure VSCode IDE** and prepare it to be used with Node.js and with AWS SAM. [Go](sections/0-ConfigVSCode)

- **Step 1. REST in (Nodejs + express) vs AWS Lambda**
    * Comparative between using **REST** in a common Nodejs stack and a Serverless. [Go](sections/1-REST)

- **Step 2. Static web (SPA) - How to deploy a regular Single Page Application the common way VS Amazon S3**
    * How to deploy a **Single Page Application** normally and how it is done in an Amazon S3 bucket.[Go](sections/2-SPADeployment)

- **Step 3. Files**
    * **Managing files** in different environments (nodejs and AWS Lambda). [Go](sections/3-FilesManagement)

- **Step 4. Databases**
    * Using (Nodejs + **MongoDB**) VS (AWS Lambda + **DynamoDB**). [Go](sections/4-Databases)

- **Step 5. Background tasks**
    * Use of **background tasks** (i.e. generate pdfs...) in both environments. [Go](sections/5-BackgroundTasks)

- **Step 6. Notifications/Websockets**
    * Creating notifications for users making use of **Websockets**. [Go](sections/6-Notifications)

- **Step 7. Users Management**
    * Users management using a typical **authentication system** in Node.js and how to do it using "AWS Cognito" instead. [Go](sections/7-UsersManagement)

- **Complete Mini App using all the features above (+ testing)**
    * Creation of a complete small app using all the features above in both stacks, also adding tests for both implementations. [Go](sections/8-CompleteMiniApp)

- **Step 9. Average cost having chosen one of this stacks**
    * Aproximation to the cost of using a typical stack VS using a Serverless one. We will see when it's prefferable to use one or another in a short/long term based on AWS prices.
        1. [Go](sections/9-CostEstimation)