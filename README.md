# 🚀 AWS Project: [AWS Serverless Sentiment Analysis](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis)

## 📌 Project Description

A lightweight, serverless web application for performing sentiment analysis on user-provided text using **AWS Comprehend**. The backend logic is handled by **AWS Lambda** and is accessible through **Amazon API Gateway**. A simple frontend interface allows users to submit text, which is then processed to determine its sentiment (e.g., Positive, Negative, Neutral, Mixed).

This project utilizes the following AWS services:
- **AWS Lambda** for serverless compute
- **Amazon API Gateway** to expose the API
- **Amazon S3** for hosting the frontend (if applicable)
- **Amazon Comprehend** for natural language processing and sentiment detection

The application is designed to be cost-effective, scalable, and easy to deploy, following a fully serverless architecture.


## 🏗️ Architecture Overview &  📹 Final Output  

#### Architecture Diagram

[![Architecture](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/Architecture_new.jpg?raw=true)](https://youtu.be/15-qhxjy140?si=Luzu1PX6CI86MQuu)

## 📦 Features

* 🔍 Analyze text sentiment (Positive, Negative, Neutral, Mixed)
* ⚡ Fast response 
* 🌐 Exposed via AWS API Gateway
* 🖥️ Simple frontend for user input
* ☁️ 100% serverless on AWS

---

## ⚙️ Technologies / Services Used

- **AWS Root Account**  
- **AWS Lambda** — Python-based serverless function using the **Boto3** SDK  
- **Amazon Comprehend** — Sentiment Detection Service  
- **Amazon API Gateway** — REST API to expose Lambda function  
- **Amazon S3** — Hosting static frontend (HTML, JS)  
- **AWS IAM** — Roles and policies for secure access management  
- **Postman** — API testing tool for verifying endpoints  

---


## 🚀 Setup Instructions

### 1. AWS S3 Bucket (Frontend - HTML + JS)

- Create an **S3 bucket** and upload the [`index.html`](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/index.html) file.
- Enable **Static Website Hosting** under the bucket's **Properties** tab.
- Set the **ACL permissions** to public to allow public access to the files.
  
  **Sample S3 Bucket Configuration:**
  ![S3 Bucket](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/S3/s3_bucket.jpg)

---

### 2. AWS Lambda (Python Backend)

- Create a **Lambda Function** named `SentimentAnalysisFunction`.
- Upload the [`lambda_function.py`](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/lambda_function.py) file as the function code.
- Use **Boto3 SDK** within Lambda to invoke AWS Comprehend.

  **Lambda Function Code Example:**
  ![Lambda Code](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/LAMBDA/lambda_code.jpg)

- Deploy the function and test to ensure it returns a successful status (`200 OK`).

  **Lambda Test Example:**
  ![Lambda Test](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/LAMBDA/lambda_test.jpg)

- Configure Lambda with:
  - **Memory:** 1280 MB
  - **Timeout:** 1 minute 3 seconds

  **Lambda Configuration:**
  ![Lambda Configuration](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/LAMBDA/lambda_config.jpg)

---

### 3. AWS IAM Roles & Permissions

Attach the following IAM policies to the Lambda execution role:

| Policy Name                                             | Type               |
|--------------------------------------------------------|--------------------|
| AmazonAPIGatewayAdministrator                          | AWS Managed        |
| AmazonAPIGatewayInvokeFullAccess                       | AWS Managed        |
| AmazonS3FullAccess                                     | AWS Managed        |
| AWSLambdaBasicExecutionRole-a9e941ff...                | Customer Managed   |
| ComprehendFullAccess                                   | AWS Managed        |
| sentiment-analysis-results-policy                      | Customer Inline    |

- View the inline policy: [`sentiment-analysis-results-policy.json`](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/sentiment-analysis-results-policy.json)

**IAM Permissions Configuration:**
![IAM Permissions](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/IAM/role(iam).jpg)

---

### 4. AWS API Gateway Configuration

- **Method:** POST  
- **Resource:** `/analyze`  
- **Integration Type:** Lambda Proxy Integration  
- **CORS:** Enabled (`*`)  

**API Gateway Setup Screenshots:**

1. **Integration Request (Lambda Proxy Enabled):**  
   ![Integration Request](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/API-GATE-WAY/api-gate-way-IR-lambda-proxy-IR-(True).jpg)

2. **Method Response Headers Configuration:**  
   ![Method Response](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/API-GATE-WAY/api-gate-way-MRes-ResHeaders3.jpg)

3. **API Testing & Deployment:**  
   ![Test & Deploy](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/API-GATE-WAY/api-gate-way-test%26-depoly.jpg)

---

### 5. API Testing with Postman

- Test the deployed API endpoint using **Postman** with a POST request.

  **Postman Testing Screenshot:**
  ![Postman Testing](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/TESTING/post-man-api-testing.jpg)

---

### 6. Final Output

- **Local System Execution:**
  ![Local Output](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/OUTPUTS/output_local-system.jpg)

- **Cloud Deployment via S3 (Static Website):**
  ![S3 Output](https://github.com/22MH1A42G1/AWS-Serverless-Sentiment-Analysis/blob/main/imgs/OUTPUTS/output_static_website.jpg)

---

## ❗ Troubleshooting

- **502 Internal Server Error**  
  Ensure that the Lambda function correctly parses the incoming request using `event['body']`. Incorrect parsing may lead to malformed responses and trigger this error.

- **CORS Errors in Browser**  
  Confirm that **CORS (Cross-Origin Resource Sharing)** is enabled in your API Gateway configuration. Allow the required headers and methods (typically `*` for development).

- **AWS Lambda Permission Issues**  
  Make sure that the **ComprehendFullAccess** policy is attached to the IAM role associated with your Lambda function. Without this, AWS Comprehend cannot be invoked from Lambda.

- **Missing Static Website URL (S3)**  
  Verify that **Static Website Hosting** is enabled in the S3 bucket properties. Also ensure that the **index document** is set (e.g., `index.html`) and that public read permissions are applied.

- **Frontend Fetch API Errors (Network Error)**  
  Confirm that you are using the **correct API Gateway Invoke URL** in your frontend JavaScript. Any mismatch (like using a wrong stage URL) will lead to fetch/network failures.

- **Lambda Timeout Exceeded**  
  If Lambda runs longer than expected and times out, increase the **timeout limit** in Lambda settings (under Configuration → General Configuration).

- **Access Denied (403) Errors in S3**  
  Ensure that **ACL permissions** are set to public or the correct bucket policy is in place to allow public access to your static files.

- **API Gateway Deployment Not Updated**  
  After making changes in API Gateway (like enabling CORS or changing integrations), don’t forget to **Deploy the API to the selected stage**. Otherwise, your changes won't take effect.

- **Boto3 Module Not Found in Lambda**  
  AWS Lambda comes with **boto3 pre-installed**. If you're deploying a custom package or layer, ensure it doesn't conflict or miss the boto3 library.

- **Invalid JSON Format in API Requests**  
  Make sure that the request body sent via the frontend or Postman is **valid JSON format**. Example:
  ```json
  {
    "text": "Your sample text here"
  }
---

## 🧑‍💻 Author

**Indana Aditya**
[LinkedIn](https://www.linkedin.com/in/aditya-indana-899734216) • [GitHub](https://github.com/22MH1A42G1)

---
