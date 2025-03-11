# AWS Lambda

## Objectives
This document covers the following key topics:

- Lambda Functions
  - Use cases
  - The programming model
  - Lambda blueprints

## 1.1 What is AWS Lambda?
AWS Lambda is a service that lets you run code in a PaaS-like environment with zero administration. 

- No EC2 instance required.
- Known as **Serverless Computing** or **NoOps**.
- AWS provisions, scales, and manages the infrastructure for running code (Lambda function).
- You pay only for the actual time your function runs (charged in 0.1-second increments).
- You specify the amount of computing memory (default: 128MB), and CPU is allocated proportionally.
- Lambda functions can:
  - Respond to AWS service events (e.g., S3 updates, HTTP requests).
  - Interact with external resources like DynamoDB or other web services.

## 1.2 Supported Languages
Supported languages as of 2017:

- **Node.js (JavaScript)**
- **Python 2.7**
- **Java 8** (Uses Amazon Linux build of OpenJDK 1.8)
- **C#** (Distributed as a NuGet package with `dotnetcore1.0` runtime parameter)

Lambda supports dependent libraries, including native ones.
- There are no white-listed APIs.
- Code can spawn additional threads if needed.
- **Disabled Activities:**
  - Inbound network connections are blocked.
  - Only TCP/IP outbound connections are supported.
  - `ptrace` (debugging) system calls are restricted.
  - TCP port 25 traffic is restricted to prevent spam.

## 1.3 Getting Your Code Up And Running
You can deploy Lambda functions in three ways:

1. **In-line coding in AWS Management Console** (Only for Node.js and Python)
2. **Local development:** ZIP/JAR bundle with dependencies uploaded to AWS.
3. **Upload ZIP/JAR to S3** and deploy from there.

### Notes:
- Maximum upload size: **50MB (compressed)**.
- AWS provides Eclipse & Visual Studio plugins for Lambda deployment.
- Use **Environment Variables** for configuration.
- For sensitive data, use **AWS KMS encryption**.

## 1.4 Example Lambda Functions
### **Node.js (ECMAScript 2015)**
```javascript
exports.handler = (event, context, callback) => {
    callback(null, 'Hello from Lambda');
};
```
### **Python**
```python
def lambda_handler(event, context):
    return 'Hello from Lambda'
```

## 1.5 Use Cases
- High-throughput real-time workflows.
- Backend processing for applications.
- Media object transcoding.
- Real-time monitoring of AWS service calls.
- Frontend HTTP service (via API Gateway).

## 1.6 How It Works
- AWS Lambda functions run inside an **AWS-managed VPC**.
- Optionally, Lambda can run within a **custom VPC**.
- Code should be **stateless**; persist state in S3, DynamoDB, etc.
- Lambda can be triggered by **event sources** (e.g., S3 file upload).
- Can also be invoked directly via **AWS SDKs** or **AWS Mobile SDKs**.
- Lambda is well-suited for **microservices architectures**.

## 1.7 Example: Processing S3 Source Events with Lambda
Example from AWS documentation demonstrating how to trigger Lambda functions on S3 events.

## 1.8 The Programming Model
Core concepts:

- **Handler:** Function that is invoked in response to events.
- **Event Object:** Data passed to the function.
- **Context Object:** Provides execution metadata (e.g., remaining execution time, log group, AWS request ID).
- **Logging:** Use `console.log` (Node.js) or `print` (Python); logs are stored in **CloudWatch Logs**.

## 1.9 Configuring Lambda Functions
AWS Management Console allows easy configuration:

- Define **event sources** that trigger Lambda execution.
- Provide function code (in-line or uploaded ZIP file).
- Assign **IAM Role** for execution.
- Set **memory size** (default: 128MB), **timeout** (3 seconds to max 5 minutes), and **environment variables**.

## 1.10 Configure Triggers
Define which events will trigger Lambda execution via AWS Console.

## 1.11 Lambda Function Blueprints
Lambda provides **pre-configured templates** for common use cases:

- **dynamodb-process-stream**: Logs updates to a DynamoDB table.
- **lex-book-trip-python**: Uses Amazon Lex for natural language understanding.
- **microservice-http-endpoint**: Simple REST API backend using API Gateway and DynamoDB.

## 1.12 Monitoring & Troubleshooting
AWS Lambda automatically tracks real-time metrics in **CloudWatch**:

- **Invocation count**
- **Invocation duration**
- **Invocation errors**

Use CloudWatch logs located at: `/aws/lambda/<function-name>`.

## 1.13 Developing Lambda in Java
- Create a **Maven project** or use the AWS **Eclipse Toolkit**.
- Add dependency: `com.amazonaws:aws-lambda-java-core`.
- Define a class implementing `RequestHandler<MyRequest, MyResponse>`.
- Example Java Lambda:

```java
public class MyLambda implements RequestHandler<MyRequest, MyResponse> {
    public MyResponse handleRequest(MyRequest request, Context context) {
        // Function logic
    }
}
```
- Build the project as a **JAR** and upload it to AWS Lambda.

## 1.14 Summary
AWS Lambda enables serverless computing and is ideal for microservices, automation, and event-driven applications.
