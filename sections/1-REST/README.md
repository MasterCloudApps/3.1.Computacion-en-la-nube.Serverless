
## Documentation
<details>
<summary>Nodejs + Express</summary>
<p>

## Documentation: Nodejs + Express
* This API provides the common methods to manage data. In this case, we can manage users.

### Requirements
- **Nodejs** [Download](https://nodejs.org/es/download/)
- A REST consumer app like **POSTMAN** [Download](https://www.getpostman.com/downloads/)

### Installation
1. Create a folder wherever you want the project to be and go inside it.
2. Go to console and, in the previous folder route, write the following command to **clone this repository** (you can download it manually too):

    ```
    git clone https://github.com/codeurjc-students/2019-ServerlessVsExpress.git
    ```

3. From the console, navigate to the folder "sections/REST/nodejs-express".
4. To install the necessary dependencies for this project, write:
    ```
    npm install
    ```
    At this point, we should have express module installed automatically, which is going to allow us to set the server and routes.

### Use
We just need to run the server and make requests. We can do that by typing:
```
node index.js
```
After doing this, a window should pop up from our default browser, but if it doesn't, you can access manually in [http://localhost:3000](http://localhost:3000). This is our REST API entry point.

#### GET requests
1. We can ask our API to **get all the users** existing in our database using the following format in our request:

    **Method**: GET <br/>
    **Route:** http://localhost:3000/users <br/>
    **Body parameters:** none <br/>

</p>
</details>

<details>
<summary>AWS Lambda</summary>
<p>

## Documentation: AWS Lambda + AWS API Gateway
* This API provides the common methods to manage data. In this case, we can manage users.

### Requirements
- **Nodejs** [Download](https://nodejs.org/es/download/)
- A REST consumer app like **POSTMAN** [Download](https://www.getpostman.com/downloads/)
- **AWS CLI and AWS SAM CLI** (You will need to have an **AWS account**).

### Installation
1. Create a folder wherever you want the project to be and go inside it.

2. Go to console and, in the previous folder route, write the following command to clone this repository (you can download it manually too):

    ```
    git clone https://github.com/codeurjc-students/2019-ServerlessVsExpress.git
    ```

3. From the console, navigate to the folder **"sections/1-REST/aws-lambda"**.

4. Build and deploy
    ```
    sam build
    sam deploy --guided
    ```

### Use

To see the route to make the api requests, we first need to go to AWS console in the browser, and then search for **Services -> API Gateway -> Select your stack name (in this case, mystack) -> Stages -> Open the Prod or Stage arrow**. There, we need to copy the **INVOKE url**, which will be something similar to this: https://xxxxxxx.execute-api.eu-west-3.amazonaws.com/Prod


#### GET requests
1. We can ask our API to **get all the users** existing in our database using the following format in our request:

    **Method**: GET <br/>
    **Route:** https://xxxxxxx.execute-api.eu-west-3.amazonaws.com/Prod/users <br/>
    **Body parameters:** none <br/>

</p>
</details>

## Comparative

### Differences based on the implementation
In **Nodejs**, we just need to define a GET resource in our Express app variable, where we will either serve our data, or return an error. The return object, can be customized the way we want. In our code, we have an example:

```javascript
app.get('/users', (req, res) => {
    services.getAllUsers()
    .then((users) => {
        res.send({
            error: false,
            code: 200,
            message: 'Users fetched successfully',
            data: users
        });
    })
    .catch((err) => {
        res.send({
            error: true,
            code: 200,
            message: err
        });
    });
    
});
```

If we try to do the same thing with **AWS Lambda**, the way to do it changes. We must define a **handler** for every Lambda function we have. In our case, in our **template.yaml** file, we have a function called **usersFunction**, where the handler points to **index.usersHandler**. We can define our handler like this, in our index file:

```javascript
exports.usersHandler = (event, context, callback) => {
    switch (event.httpMethod) {
        case 'GET':
            getAllUsers(callback);
            break;
        default:
            sendResponse(400, `Unsupported method ${event.httpMethod}`, callback);
    }
};
```

As you can see, there isn't any route to get all the users in the javascript file. That's because we need to create an event for it in template.yaml:

```YAML
Events:
    lambdaGetAllUsers:
        # Define an API Gateway endpoint that responds to HTTP GET at /users
        Type: Api
        Properties:
            Path: /users
            Method: GET
```

One more noticeable difference is that in **AWS Lambda**, we must return always a **response with two attributes**, a statusCode (integer), and a body (it must be a JSON object):

```javascript
const sendResponse = (statusCode, message, callback) => {
    const res = {
        statusCode: statusCode,
        body: JSON.stringify(message)
    };
    callback(null, res);
};
```

We can also see that instead of a send method, as the one we had in Express, we have now a **callback()**, where the response is sent in the second parameter.

One last, but not less importat thing, is that, by default, we don't need to specify **permissions** to access the endpoints. **By default, they are public**. We will see how to filter by roles and more options in further sections.

### Differences based on the functionality
When it comes to functionality, they behave kind of different. They both serve the data if requested, but if there is a use case where the server has to **attend too many requests**, the nodejs stack could lead to a **bottleneck**, stopping some of its functionality, while in the AWS Lambda stack, as we have our **endpoint wrapped in a Lambda function**, it could **scale easily**, replicating the lambda if necessary, allowing us to keep our event listeners working. 

The Lambda functions behaviour also implies that if we need to save something in non-persistent memory, this **data will not exist once the lambda is replicated**, because it would run under a **different Runtime environment**. That means, if we would like to create a full CRUD API, we would need a database to keep or data safe.