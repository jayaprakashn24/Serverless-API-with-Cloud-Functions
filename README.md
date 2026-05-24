# Serverless API Setup: API Gateway + Lambda + DynamoDB
![alt text](<Serverless Application Using AWS Lambda + API Gateway + DynamoDB-1.png>)
This guide explains how to create a simple serverless API using:

* Amazon API Gateway
* AWS Lambda
* Amazon DynamoDB

---

# Architecture Overview

```text
Client
   ↓
API Gateway
   ↓
Lambda Function
   ↓
DynamoDB
```

---

# 1. Create a DynamoDB Table

1. Open the AWS Management Console.
2. Navigate to DynamoDB.
3. Click **Create table**.
4. Enter the following details:

   * **Table name:** `cloud`
   * **Partition key:** `email` (String)
5. Click **Create table**.

---

# 2. Create an IAM Role for Lambda

1. Go to IAM → Roles.
2. Click **Create role**.
3. Select:

   * **Trusted entity type:** AWS Service
   * **Use case:** Lambda
4. Attach the required permissions policy.

> Recommended: Use least-privilege access instead of `AdministratorAccess`.

For example, attach:

* `AWSLambdaBasicExecutionRole`
* A custom DynamoDB access policy (or `AmazonDynamoDBFullAccess` for testing)

5. Enter the role name:

   * `Lambda-Role`
6. Click **Create role**.

---

# 3. Create a Lambda Function

1. Go to Lambda → **Create function**.
2. Choose **Author from scratch**.
3. Configure the function:

   * **Function name:** `crackone`
   * **Runtime:** Python 3.13

4. Click **Create function**.

## Configure Timeout

1. Open the Lambda function.
2. Go to **Configuration → General configuration → Edit**.
3. Set:

   * **Timeout:** `15 minutes`
4. Save the changes.

## Upload Code

1. Go to the **Code** tab.
2. Upload or paste your Lambda function code.
3. Deploy the changes.

---

# 4. Create an API Gateway REST API

1. Go to API Gateway → **Create API**.
2. Choose **REST API** → **Build**.
3. Enter:

   * **API name:** `myapi`
4. Click **Create API**.

---

# 5. Create API Methods

## Create a GET Method

1. Select the root resource (`/`).
2. Click **Create Method**.
3. Choose:

   * **Method type:** `GET`
4. Configure:

   * **Integration type:** Lambda Function
   * Enable **Lambda proxy integration**
5. Select your Lambda function (`crackone`).
6. Save the configuration.

---

## Create a POST Method

1. Click **Create Method** again.
2. Choose:

   * **Method type:** `POST`
3. Configure:

   * **Integration type:** Lambda Function
   * Enable **Lambda proxy integration**
4. Select the same Lambda function.
5. Save the configuration.

---

# 6. Deploy the API

1. Click **Deploy API**.
2. Create a new stage:

   * **Stage name:** `dev`
3. Click **Deploy**.

After deployment, copy the **Invoke URL**, for example:

```text
https://xxxxxx.execute-api.ap-south-1.amazonaws.com/dev
```

You can now access your API using:

```text
GET  https://xxxxxx.execute-api.ap-south-1.amazonaws.com/dev
POST https://xxxxxx.execute-api.ap-south-1.amazonaws.com/dev
```

---

