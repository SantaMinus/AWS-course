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

### Example: Orchestrating Lambda functions with Step Functions

![lambda-step-functions.jpg](images%2Flambda-step-functions.jpg)

With this model, the Lambda functions can be focused on the business logic, letting Step Functions control the sequence
and timing of each task and track the state of the workflow.

You can orchestrate any AWS service using **Step Functions** service integrations. **Service integrations** allow you to
create calls to AWS services and include the response into your workflow.

AWS SDK service integrations allow you to invoke one of over 9,000 AWS API actions from over 200 services directly from
your Step Functions workflow.

### Enterprise messaging patterns supported in Step Functions

**Sequential tasks**: Iterates through each state
in your state machine in sequential order
![sequential-tasks-pattern.png](images%2Fsequential-tasks-pattern.png)

**Conditional choice:** Adds branching logic to your state machine
![conditional-choise-pattern.png](images%2Fconditional-choise-pattern.png)

**Looping tasks**: Iterates on your state machine task a specific number of times

![loooping-task-pattern.png](images%2Floooping-task-pattern.png)

**Try/catch/finally:** Deals with errors and exceptions automatically based on your defined business logic
Parallel: Used to create parallel branches in your state machine

![try-catch-finally-pattern.png](images%2Ftry-catch-finally-pattern.png)

**Parallel:** Used to create parallel branches in your state machine

![parallel-pattern.png](images%2Fparallel-pattern.png)

> A common use case for Step Functions is tasks that requires human intervention, such as a manual approval process in a
> workflow. To implement this, you can use the **callback integration pattern** with the enterprise messaging patterns
> to
> call a service with a task token. Step Functions will wait until that task token is returned with a payload before
> progressing with the workflow.

### Step Functions Express Workflows

Step Functions Express Workflows are an alternative to the Standard Workflows. Express Workflows are designed for
high-volume, short-duration use cases.

## Patterns for Communicating Status Updates

### Three patterns for communicating status updates

In the previous example, the client service submitted an order, and the SQS queue gave a response to API Gateway with
the message ID. Then Lambda pulled the message from the queue and handed it off to Step Functions to complete the order
process and update the job status as it moved through the workflow.

The next thing you need to consider is **how to provide the status to the calling client**. The method you choose
depends on the use case. There are three potential approaches:

- **Client polling**
- Webhooks with Amazon Simple Notification Service (Amazon SNS)
- WebSockets with AWS AppSync

### Client polling

Client poling is a common way to get status information on a long-running transaction.

The client can use the ID returned from Amazon SQS to get the status of the request.
![client-polling.png](images%2Fclient-polling.png)

**Advantages:**

- Convenient to replace a synchronous flow

**Disadvantages:**

- Adds additional latency to the consistency of the data to the client
- Unnecessary work and increased cost for requests and responses when nothing has changed

### Webhook pattern with Amazon SNS

Webhook pattern with Amazon SNS

Another way to improve on the polling approach is to use webhooks. **Webhooks** are user-defined HTTP callbacks.

### Example: Using trusted webhooks with Amazon SNS

With trusted clients, you can use Amazon SNS to set up an HTTP subscriber that notifies the client using the webhook. An
added benefit to using Amazon SNS with HTTP subscribers is that it can model retry behaviors and exponential back-offs
until the webhook succeeds.

![webhook-example.jpg](images%2Fwebhook-example.jpg)

Amazon SNS sends a message to its topic subscribers, using filtering to target the right subscriber. The message
includes the information about the order that the client service needs.

### WebSockets pattern with AWS AppSync

**WebSocket APIs** are an open standard used to create a persistent connection between the client and the backing
service, permitting **bidirectional** communication. You can provide the status for the client using WebSockets with
GraphQL subscriptions to listen for updates through AWS AppSync.

**GraphQL** is a query language for APIs and a runtime for fulfilling those queries.

AWS AppSync is a fully managed GraphQL service with real-time data synchronization and offline programming features.

In the order submission example:

- The client creates a subscription for order status updates and submits the order through the AppSync service.
- As data changes, the client receives updates through the subscription. This includes changes related to the status of
  the work as well as changes to the order data.

This is a simpler architecture than using API Gateway with a combination of REST and WebSocket APIs. As with the
previous example, you need to understand and monitor the number of client requests. The direct AWS AppSync to Step
Functions integration means that requests that exceed the Step Functions StartExecution API limit will be throttled.

| AWS AppSync                                      | Amazon API Gateway                                                |
|--------------------------------------------------|-------------------------------------------------------------------|
| Greatly reduces the number of API calls required | Fine-grained management of APIs                                   | 
| Lets client filter only the data it needs        | More existing **industry best practices** and **field knowledge** |

### Example: Using WebSockets with AWS AppSync

With **AWS AppSync**, clients can automatically subscribe and receive status updates as they occur. This is a great
pattern when **data drives the user interface**, and is ideal for **data that is streaming** or may yield **more than
a single response**.

![web-sockets.jpg](images%2Fweb-sockets.jpg)

## Serverless Data Processing Patterns

### Data processing with Amazon Kinesis

You can use **Kinesis** streaming services to ingest and process large volumes of data in near-real time.

### Amazon Kinesis Data Streams

Amazon Kinesis Data Streams

Clickstream data from mobile apps, application logs, and home security camera feeds are common examples where
traditional batch processing would not give you the responsiveness you need to make data actionable. The stream also
provides an asynchronous data buffer for downstream processing.

![kinesis-data-streams.jpg](images%2Fkinesis-data-streams.jpg)

**Producers** add data records to the stream.

Producers can be **created** with the following:

- Kinesis Producer Library (KPL)
- AWS SDK
- Third-party tools

**Consumers** get records and process them.

Consumer can be any of the following:

- Lambda functions
- Other streams
- Applications created with Kinesis Client Library (KCL)

### Kinesis Data Streams capacity limits (per shard)

Kinesis Data Streams scales horizontally by adding **shards**. Each shard has a uniquely identified sequence of data
records, and each data record has a sequence number assigned by Kinesis.

|            | Writes (Putting Data on the Stream) | Reads (Consuming Data off the Stream)           |
|------------|-------------------------------------|-------------------------------------------------|
| Rate Limit | 1,000 records per second            | 5 transactions per second, up to 10,000 records |
| Data Limit | 1 MB per second                     | 2 MB per second                                 |

### Example: Serverless data processing flow with Kinesis Data Firehose

With Amazon Kinesis Data Firehose, you don't need to write consumer applications or manage shards. You configure your
data producers to send data to **Kinesis Data Firehose**, and it automatically delivers the data to a destination that
you specify.

![kinesis-data-firehose.jpg](images%2Fkinesis-data-firehose.jpg)

### Example: Serverless data processing flow with Kinesis Data Analytics

You can also update the processing stream to include a **Kinesis Data Analytics** application before storing the data to
Amazon Simple Storage Service (Amazon S3). Kinesis Data Analytics lets you do **real-time analysis using SQL** before
persisting the data.

![kinesis-data-analytics.gif](images%2Fkinesis-data-analytics.gif)

### Kinesis Data Streams and Kinesis Data Firehose

| Kinesis Data Streams                                                | Kinesis Data Firehose                                                    |
|---------------------------------------------------------------------|--------------------------------------------------------------------------|
| You write custom consumers, with more targets                       | The use case of transforming and storing streaming data is simplified    |
| Order delivery and exactly-once delivery are guaranteed             | Order not guaranteed; messages could be delivered more than once         |
| Failing messages **block** the shard until it succeeds or expires   | **Retry mechanisms** are available for each delivery target              |
| You set the **number of shards**                                    | You set the **data volume** and the service manages the number of shards |
| **Multiple consumers** and multiple **types** of consumers are used | Stream is associated with a **single destination**                       |

### Amazon SNS filtering, fan-out, and nested serverless applications

Another serverless data processing pattern you can use is **messaging**, instead of streaming. Amazon **Simple
Notification Service** (Amazon SNS) uses a publication/subscription, or pub/sub, model, which means that a single
published message can have multiple consumers.

### Amazon SNS message filtering

In the Amazon SNS message filtering pattern, the subscriber assigns a **filter policy** to the topic subscription. The
filter policy contains attributes that define which messages that subscriber receives, and Amazon SNS compares message
attributes to the filter policy for each subscription. If the attributes don’t match, the message doesn’t get sent to
that subscriber.

![sns-message-filtering.jpg](images%2Fsns-message-filtering.jpg)

### Nested serverless applications

Tasks like data backup or indexing data for search or analytics repeat across your larger applications. You can build
these recurring patterns as smaller serverless applications and reuse them.

![nested-serverless-application.jpg](images%2Fnested-serverless-application.jpg)

### Amazon EventBridge

**Amazon EventBridge** is a serverless **event bus** that facilitates connection of applications together using AWS
services and data from your own applications, as well as integrated software-as-a-service (SaaS) applications.

### Streaming or messaging for data processing

| Messaging                                                             | Streaming                                                                                                                   |
|-----------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| The core entity is an **individual message**, and message rates vary. | You look at the **stream of messages** together, and the stream is generally continuous.                                    |
| Messages are **deleted** when they’ve been consumed.                  | Data **remains on the stream** for a period of time. Consumers must maintain a pointer.                                     |
| Configure **retries** and **dead-letter queues** for failures.        | A message is **retried until it succeeds or expires**. You must build error handling into your function to bypass a record. |

## Failure Management in Event-Driven Architectures

### Failure management in your functions

### Error handling for synchronous and asynchronous events

**Errors**:

- **function** (Lambda successfully hands off an event to your function, but the function throws an error or times out
  before completing)
- **invocation** (the request is rejected before your function receives it. An oversized payload or lack of permissions
  will
  cause an invocation error)

In a **synchronous invocation**, like between **API Gateway** and Lambda, no retries are built in. You have to write
code to handle errors and retries for all error types.

With **asynchronous** event sources, like **Amazon S3**, Lambda provides **built-in retry behaviors**.

When Lambda gets an **asynchronous** event, it handles it a lot like the **SQS** example earlier. The Lambda service
returns a success to the event source and puts the request in **its own internal queue**. Then it sends invocation
requests from its queue to your function.

If the invocation request returns invocation errors, Lambda **retries** that request **two more times by default**. You
can configure this retry value between **0 and 2**.

If the invocation request returns invocation errors, Lambda **retries** that invocation **for up to 6 hours**. You can
decrease the default duration using the maximum age of event setting.

To handle events that continue to fail beyond the retry or maximum age settings, you can configure a **dead-letter queue
** or a **failed-event destination** on the Lambda function.

### Error handling for stream-based events

**Stream-based event sources** (like Kinesis Data Streams or DynamoDB Streams) need to **maintain record order per
shard**. So, by default, if an error occurs while Lambda is processing a batch of records, Lambda won’t process any new
records from that shard until the batch succeeds or expires. You can use the **Iterator-Age** metric to detect blocked
shards.

Configuration options (since 2019):

- **Bisect batch on function error** (split a failing batch into two and retry each batch separately)
- **Maximum retry attempts** (limit the number or duration of retries on a failed batch)
- **Maximum record age** (limit the number or duration of retries on a failed batch)
- **On-failure destination** (send failed records to an SNS topic or SQS queue for offline processing without adding
  function logic)

These options provide **flexible error handling**, but they also introduce the potential for a record to be **processed
multiple times**.

This means you have to **handle idempotency** in your function rather than assuming only-once record processing.

#### Advantages of on-failure destination:

1. Additional data
2. More flexible

Can also specify **on-success** destination.

### Error handling with Amazon SQS as an event source

For polling event sources that aren’t stream based, for example **Amazon SQS**, if an invocation fails or times out, the
message is available again when the **visibility timeout period** expires.

Lambda keeps retrying that message until it is either successful or the queue’s **Maxreceivecount** limit has been
exceeded.

It’s a best practice to set up a **dead-letter queue** on the source queue to process the failed messages.

**Parameters to set:**

- Lambda function timeout
- Visibility timeout
- Batch size

The maximum timeout for Lambda is 15 minutes. The **best practice** is to set your **visibility timeout** to **6 times
the timeout** you configure for your **function**.

### Error handling summary by execution model

#### API Gateway (synchronous event source)

- **Timeout considerations** – API Gateway has a 30-second timeout. If the Lambda function hasn't responded to the
  request within 30 seconds, an error is returned.
- **Retries** – There are no built-in retries if a function fails to execute successfully.
- **Error handling** – Generate the SDK from the API stage, and use the backoff and retry mechanisms it provides.

#### Amazon SNS (asynchronous event source)

- **Timeout considerations** – Asynchronous event sources do not wait for a response from the function's execution.
  Requests are handed off to Lambda, where they are queued and invoked by Lambda.
- **Retries** – Asynchronous event sources have built-in retries. If a failure is returned from a function's execution,
  Lambda will attempt that invocation two more times for a total of three attempts to execute the function with its
  event payload. You can use the Retry Attempts configuration to set the retries to 0 or 1 instead.

  If Lambda is unable to invoke the function (for example, if there is not enough concurrency available and requests are
  getting throttled), Lambda will continue to try to run the function again for up to 6 hours by default. You can modify
  this duration with Maximum Event Age.

  Amazon SNS has unique retry behaviors among asynchronous events based on its delivery policy for AWS Lambda(opens in a
  new tab). It will perform 3 immediate tries, 2 at 1 second apart, 10 backing off from 1 second to 20 seconds, and
  100,000 at 20 seconds apart.
- **Error handling** – Use the Lambda destinations(opens in a new tab) OnFailure option to send failures to another
  destination for processing. Alternatively, move failed messages to a dead-letter queue on the function. When Amazon
  SNS is the event source, you also have the option to configure a dead-letter queue on the SNS subscription(opens in a
  new tab).

#### Kinesis Data Streams (polling a stream as an event source)

- **Timeout considerations** – When the retention period for a record expires, the record is no longer available to any
  consumer. The retention period is 24 hours by default. You can increase the retention period at a cost.

As an event source for Lambda, you can configure **Maximum Record Age** to tell Lambda to skip processing a data record
when it has reached its Maximum Record Age.

- **Retries** – By default, Lambda retries a failing batch until the retention period for a record expires. You can
  configure **Maximum Retry Attempts** so that your Lambda function will skip retrying a batch of records when it has
  reached the Maximum Retry Attempts (or it has reached the Maximum Record Age).
- **Error handling** – Configure an OnFailure destination on your Lambda function so that when a data record reaches the
  Maximum Retry Attempts or Maximum Record Age, you can send its metadata, such as shard ID and stream Amazon Resource
  Name (ARN), to an SQS queue or SNS topic for further investigation.

  Use **BisectBatchOnFunctionError** to tell Lambda to split a failed batch into two batches. Retry your function
  invocation with smaller batches to isolate bad records and work around timeout and retry issues.

#### SQS queue (polling a queue as an event source)

- **Timeout considerations** – When the visibility timeout expires, messages become visible to other consumers on the
  queue. Set your visibility timeout to 6 times the timeout you configure for your function.
- **Retries** – Use the **maxReceiveCount** on the queue's policy to limit the number of times Lambda will retry to
  process a failed execution.
- **Error handling** – Write your functions to delete each message as it is successfully processed. Move failed
  messages to a dead-letter queue configured on the source SQS queue.

## Failure management across the application

There are ways that these four AWS services can provide failure management across your application:

- AWS Step Functions
- Amazon SQS
- Amazon SNS
- AWS X-Ray

### Step Functions for failure management

