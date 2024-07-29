Overview
========

A serverless URL shortener that will turn long URLs into shorter ones.

Architecture
============

### Lambda

A Python function handles the creation of short URLs and the redirection of them to their corresponding long URL.

### API Gateway

Makes the Lambda function accessible via HTTP.

### DynamoDB

Stores the mapping between long and short URLs.

Walkthrough
===========

### 1\. Deploy it

Create a CloudFormation stack using the template, `cloudformation-template.yaml`.

Give the stack any reasonable name, e.g. `url-shortener`.

### 2\. Get the API URL

After the stack is deployed, copy the API URL from the stack’s output in the CloudFormation section of the AWS console.

It will look like [https://\<api-id>.execute-api.\<region>.amazonaws.com/prod](https://<api-id>.execute-api.<region>.amazonaws.com/prod).

### 3\. Submit a long URL

This would be done via a frontend web page but until that’s created, it can be tested with [Postman](https://www.postman.com/), like so:

1.  Open Postman and create a new `POST` request.
    
2.  Set the URL to the API URL, copied in step `2` above.
    
3.  In the Body tab, select `raw` and `JSON` format.
    
4.  Enter the following JSON:
    ```
    {
        "long\_url": "https://www.example.com/this/is-a/really-long-url"
    }
    ```
5.  Send the request. You should receive a response with the shortened URL. Copy it.
    

### 4\. Test the redirect

Using your web browser:

1.  Enter the short URL, copied in step `3.V` above.
    
2.  You should be redirected to the original long URL.
    

Possible Enhancements
=====================

### Frontend

Add a simple frontend web page using S3 and CloudFront for submitting the long URL to be shortened.

### Custom domain

Currently the long API Gateway domain name of the API URL makes the resulting short URL not very short. Use Route 53 to replace this with a shorter custom domain name, such as the company’s internal domain name.

### Authentication

Secure the API by using Cognito User Pools as an authorizer of the API Gateway. Then users must register and login to the user pool in order to submit a long URL.