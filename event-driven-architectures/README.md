# Thinking Serverless

## Serverless architectures using event-driven patterns

> _Resource: “Enterprise Integration Patterns: Designing, Building and Deploying Messaging Solutions” Gregory Hohpe &
Bobby Woolf_

## Serverless Event Submission Patterns

### Amazon SQS integration with Lambda

**Amazon SQS** supports both **standard** and First-In-First-Out (**FIFO**) queues.

Standard queues provide much higher throughput, but order is not guaranteed. Also, **messages may be delivered more than
once**, so you need to write idempotent functions that can process the same message multiple times without unexpected
results.

### SQS queue between API Gateway and Lambda

By introducing the SQS queue in front of Lambda, you create an asynchronous connection between the API request and
Lambda. This allows you to satisfy the API request without regard for how long the Lambda function (or other backend
services) will run.

![sqs-lambda.jpg](images%2Fsqs-lambda.jpg)

> You can use both standard and First-In-First-Out (FIFO) SQS queues as a Lambda event source.

#### Standard vs. FIFO SQS queues as Lambda event sources

|                        | Standard                                 | FIFO                                                                                                       |
|------------------------|------------------------------------------|------------------------------------------------------------------------------------------------------------|
| Record Order           | Order is not guaranteed                  | Order is guaranteed per group ID                                                                           |
| Delivery               | Messages may be delivered more than once | Messages are delivered only once. There are no duplicate messages introduced to the queue                  |
| Transaction Throughput | Nearly unlimited messages per second     | FIFO queues support up to 300 messages per second, per API action without batching, or 3,000 with batching |

## Workflow orchestration with AWS Step Functions

**AWS Step Functions** can coordinate the distributed components of your application and keep the need for orchestration
out of your code. It automatically launches and tracks each step, and can retry steps when there are errors. 
