# AWS Lambda + DynamoDB Student Manager

A small serverless student management application built with **AWS Lambda**, **Amazon DynamoDB**, **IAM**, **Lambda Function URLs**, and **Python**.

The application provides a browser-based interface that can:

- Save a fictional student record
- Find a student by ID
- Update an existing student
- Store the data in Amazon DynamoDB
- Run entirely with serverless AWS services

## Architecture

```text
Browser
   |
   | HTTPS
   v
Lambda Function URL
   |
   v
AWS Lambda (Python)
   |
   | IAM permissions
   v
Amazon DynamoDB
```

## Technologies Used

- AWS Lambda
- Amazon DynamoDB
- AWS IAM
- Lambda Function URL
- Python 3.12
- boto3
- HTML/CSS embedded in the Lambda function
- AWS Console

## Project Configuration

| Component | Configuration |
|---|---|
| Lambda function | `mcit-student-ui` |
| DynamoDB table | `MCITStudents` |
| Partition key | `student_id` |
| Runtime | Python 3.12 |
| Region | `us-east-1` |
| Function URL authentication | `NONE` |
| Invoke mode | `BUFFERED` |
| Timeout | 10 seconds |
| Memory | 128 MB |

## Implementation

### 1. Create the DynamoDB Table

I created an Amazon DynamoDB table named `MCITStudents`.

The table uses:

- `student_id` as the partition key
- String as the key type
- On-demand capacity mode


### 2. Create the AWS Lambda Function

I created a Python Lambda function named `mcit-student-ui`.

The function contains the backend logic and also returns the HTML/CSS used by the browser interface.


### 3. Deploy the Python Application Code

The Lambda function uses `boto3` to communicate with DynamoDB.

The application supports:

- `GET` requests to retrieve a student
- `POST` requests to create or update a student
- Input validation
- UTC timestamps using `updated_at`
- HTML output returned directly from Lambda


### 4. Configure Environment Variables

The Lambda function uses environment variables so the table name and AWS region are not hard-coded into the application logic.

```text
TABLE_NAME=MCITStudents
TABLE_REGION=us-east-1
```


### 5. Configure IAM Permissions

The Lambda execution role was given permission to access the `MCITStudents` DynamoDB table.

The policy allows the application to perform the required DynamoDB operations, including reading and writing student records.

Example permissions:

```json
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:GetItem",
    "dynamodb:PutItem",
    "dynamodb:UpdateItem",
    "dynamodb:Scan"
  ]
}
```

### 6. Create the Lambda Function URL

A Lambda Function URL was configured to make the application accessible from a web browser.

Configuration:

- Authentication: `NONE`
- Invoke mode: `BUFFERED`
- CORS: disabled for this same-origin demo


> **Security note:** This was a classroom/demo application using fictional data. A public production application should use authentication and more restrictive access controls.

### 7. Save a Student

The browser UI allows a user to enter:

- Student ID
- Name
- Course

After saving, the application displays a success message and stores the record in DynamoDB.

Example test record:

```text
Student ID: s002
Name: Seb Student
Course: Cloud computing
```


### 8. Verify the DynamoDB Record

The saved student can be viewed directly inside the `MCITStudents` table.

Stored attributes include:

- `student_id`
- `name`
- `course`
- `updated_at`


### 9. Find a Student

The application can retrieve an existing student by ID and populate the saved information back into the browser form.


## What I Learned

This project helped me practice how multiple AWS services work together in a simple serverless architecture.

Key concepts included:

- Creating and configuring a DynamoDB table
- Using partition keys to identify records
- Creating and deploying an AWS Lambda function
- Using Python and `boto3` with DynamoDB
- Configuring IAM permissions for service access
- Using environment variables for application configuration
- Publishing a Lambda function through a Function URL
- Handling HTTP `GET` and `POST` requests in Lambda
- Saving, retrieving, and updating database records
- Verifying data directly in DynamoDB
- Understanding the request flow between a browser, Lambda, IAM, and DynamoDB

## Result

The final result is a working serverless web application where users can save, find, and update fictional student records without managing a traditional web server.

---

**Project type:** Cloud Computing / Serverless AWS Lab  
**Purpose:** Educational lab and portfolio project
