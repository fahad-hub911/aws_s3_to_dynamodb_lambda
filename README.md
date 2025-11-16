# aws_s3_to_dynamodb_lambda
storing the data automatically in DynamoDB
# S3 → Lambda → DynamoDB Metadata Pipeline  
Store every S3 upload automatically in DynamoDB

---

## 📌 Overview

This project provides an AWS Lambda function that listens for S3 uploads and automatically stores file metadata into a DynamoDB table.  

It is ideal for:

- WordPress media integrations  
- File tracking systems  
- Serverless ETL workflows  
- Audit logs for uploads  

---

## 🧠 Architecture

 ┌───────────┐      triggers      ┌───────────────┐
 │           │  ───────────────▶  │               │
 │    S3     │                    │    Lambda     │
 │ (Uploads) │  ◀───────────────  │  s3→dynamodb  │
 └───────────┘     reads meta     └──────┬────────┘
                                          │ writes
                                    ┌─────▼────────┐
                                    │ DynamoDB      │
                                    │ wordpress-    │
                                    │ table-data    │
                                    └───────────────┘

---

## 📂 Project Structure
aws-s3-to-dynamodb-lambda/
│
├── lambda_function.py # Main Lambda handler
├── README.md # Documentation
│
├── infra/
│ ├── terraform/ # Optional IaC
│ └── cloudformation/
│
└── .github/
└── workflows/
└── deploy-lambda.yml # CI/CD for auto-deploy


---

## 🚀 Deployment Steps

### 1️⃣ Create DynamoDB table
- Table name: `wordpress-table-data`
- Partition key: `fileName` (String)

### 2️⃣ Set Lambda environment variable
| Key | Value |
|-----|-------|
| `DYNAMODB_TABLE` | `wordpress-table-data` |

### 3️⃣ Create IAM role  
Needs:
- `s3:GetObject`
- `s3:HeadObject`
- `dynamodb:PutItem`
- `AWSLambdaBasicExecutionRole`

### 4️⃣ Add S3 Trigger  
Event: **PUT (Object Created)**

---

## 📡 WordPress API Support

This repo includes a WordPress plugin that exposes DynamoDB data via:

#/wp-json/media/v1/files

---

## 📝 License
MIT License  
