# compose-mongodb-helloworld-nodejs Overview

compose-mongodb-helloworld-nodejs is a sample IBM Cloud application that shows you how to connect to an IBM Compose for MongoDB for IBM Cloud service using Node.js.

## Running the app on IBM Cloud

1. If you do not already have an IBM Cloud account, [sign up here][IBMCloud_signup_url]

2. [Download and install IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

  The IBM Cloud CLI tool tool is what you'll use to communicate with IBM Cloud from your terminal or command line.

3. Connect to IBM Cloud in the command line tool and follow the prompts to log in.

  ```
  ibmcloud login
  ```

  **Note:** If you have a federated user ID, use the `bx login --sso` command to log in with your single sign on ID.

4. Make sure you are targeting the correct IBM Cloud org and space.

  ```
  ibmcloud target --cf
  ```

  Choose from the options provided. If you have already created the service, use the same options here as you used when creating the service.

5. Create your service.

  If you don't already have a Compose for MongoDB service in IBM Cloud, you can create one now using the `create-service` command.

  **Note :** The Compose for MongoDB service does not offer a free plan. For details of pricing, see the [Pricing](https://console.bluemix.net/docs/services/ComposeForMongoDB/pricing.html) page of the Compose for MongoDB documentation.

  You will need to specify the service plan that your service will use, which can be _Standard_ or _Enterprise_. This readme file assumes that you will use the _Standard_ plan. To use the _Enterprise_ plan you will need to create an instance of the Compose Enterprise service first. Compose Enterprise is a service which provides a private isolated cluster for your Compose databases. For information on Compose Enterprise and how to provision your app into a Compose Enterprise cluster, see the [Compose Enterprise for IBM Cloud help](https://console.bluemix.net/docs/services/ComposeEnterprise/index.html).

  To create your service, use the `create-service` command, specifying the service identifier, `compose-for-mongodb`, the service plan and a name for your new service instance. For example, to create a service called "my-compose-for-mongodb-service" on the _Standard_ plan, the command would be:

  ```
  ibmcloud cf create-service compose-for-mongodb Standard my-compose-for-mongodb-service
  ```

  The actual provisioning of the service can take a few minutes, so while that's happening you can clone the sample app, update it so it is ready to be bound to your service, and push the app to IBM Cloud.

6. Clone the app to your local environment from your terminal using the following command:

  ```
  git clone https://github.com/IBM-Cloud/compose-mongodb-helloworld-nodejs.git
  ```

7. `cd` into this newly created directory. The code for connecting to the service, and reading from and updating the database can be found in `server.js`. See [Code Structure](#code-structure) and the code comments for information on the app's functions. There's also a `public` directory, which contains the html, style sheets and javascript for the web app. For now, the only file you need to update is the application manifest.

8. Update the `manifest.yml` file.

  - Change the `name` value to something unique. The value you choose will be the name of the app as it appears in your IBM Cloud dashboard.
  - Change the `route` value. This will be the sub-domain your app will appear on. This could simply be the app name plus `.mybluemix.net`, or if in another region, `.regionid.mybluemix.net` e.g. `.eu-gb.mybluemix.net`.

  Update the `service` value in `manifest.yml` to match the name of your service.

9. Push the app to IBM Cloud. When you push the app it will automatically be bound to the service.

  ```
  ibmcloud cf push
  ```

Your application is now running at `<host>.mybluemix.net`.

The node-mongodb-helloworld app displays the contents of an _examples_ database. To demonstrate that the app is connected to your service, add some words to the database. The words are displayed as you add them, with the most recently added words displayed first.

## Code Structure

| File | Description |
| ---- | ----------- |
|[**server.js**](server.js)|Establishes a connection to the MongoDB database using credentials from VCAP_ENV and handles create and read operations on the database. |
|[**main.js**](public/javascripts/main.js)|Handles user input for a PUT command and parses the results of a GET command to output the contents of the MongoDB database.|

The app uses a PUT and a GET operation:

- PUT
  - takes user input from [main.js](public/javascript/main.js)
  - uses the `db.collection.insertOne()` method to add the user input to the database

- GET
  - uses `db.collection.find()` method to retrieve the contents of the _words_ collection
  - returns the response of the database command to [main.js](public/javascript/main.js)



[compose_for_mongodb_url]: https://console.bluemix.net/catalog/services/compose-for-mongodb/
[IBMCloud_signup_url]: https://ibm.biz/compose-for-mongodb-signup

