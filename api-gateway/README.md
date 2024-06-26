# Amazon API Gateway for Serverless Applications

**API Gateway** is a service that facilitates the creation, publishing, maintenance, monitoring, and security of your
APIs at any scale. API Gateway handles traffic management, Cross Origin Resource Sharing (CORS) support,
authorization and access control, throttling, monitoring, and API version management.

### Developer features in API Gateway

- Run multiple versions of an API at the same time
- Quick SDK generation
- Transform or validate request-response data

### Features for managing API access

- Reduce latency and throttle traffic
- Built-in, flexible authorization options
- API keys for third-party developers

![api-gateway-architecture.jpg](images%2Fapi-gateway-architecture.jpg)

## Selecting the best API type for your use case

### REST API

**REST** stands for **representational state transfer**. REST defines a set of functions such as GET, PUT, and DELETE
that clients can use to access server data. Clients and servers exchange data using HTTP.

The main feature of a REST API is **statelessness**. Statelessness means that **servers do not save client data between
requests**.

REST APIs offer API proxy functionality and API management features in a single solution. REST APIs also offer API
management features such as usage plans, API keys, publishing, and monetizing APIs.
REST API architecture showing use case of an API Gateway private endpoint connected to a VPC through an Interface VPC
endpoint.

![rest-api.png](images%2Frest-api.png)

### HTTP API

With **Hypertext Transfer Protocol** (HTTP) APIs, you can create RESTful APIs with lower latency and lower cost than
REST APIs. You can use HTTP APIs to send requests to Lambda functions or to any routable HTTP endpoint.

HTTP APIs are optimized for building APIs that proxy to Lambda functions or HTTP backends, making them ideal for
serverless workloads. They **do not** currently **offer API management functionality**.

![http-api.png](images%2Fhttp-api.png)

### Websocket API

**WebSocket API**s offer APIs that the client can access through the WebSocket protocol. Unlike REST and HTTP APIs,
WebSocket APIs allow **bidirectional communications**. WebSocket APIs are often **used in real-time applications** such
as chat applications, collaboration platforms, multiplayer games, and financial trading platforms.

WebSocket APIs maintain a persistent connection between connected clients to facilitate real-time message communication.
With WebSocket APIs in API Gateway, you can define backend integrations with Lambda functions, Amazon Kinesis, or any
HTTP endpoint to be invoked when messages are received from the connected clients.

![websocket-api.png](images%2Fwebsocket-api.png)

| REST API                                                                                        | HTTP API                                                                          | WebSocket API                                                                                                                         |
|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| All-inclusive set of features needed to build, manage, and publish APIs at a single price point | Building modern APIs that are equipped with native OIDC and OAuth 2 authorization | Bidirectional communication that lets clients and services independently send messages to each other                                  |                             
| Building APIs that use certificates for backend authentication, AWS WAF, or resource policies   | Building proxy APIs for Lambda or any HTTP endpoint                               | Richer client-to-service interactions because services can push data to clients without requiring clients to make an explicit request |
| Workloads that need an edge-optimized or private API type                                       | APIs for latency-sensitive workloads                                              | APIs for real-time communication                                                                                                      |

**REST API**s are intended for APIs that require API proxy functionality and API management features in a single
solution. **HTTP API**s are optimized for building APIs that proxy to Lambda functions or HTTP backends, making them
ideal for serverless workloads. HTTP APIs are a cheaper and faster alternative to REST APIs, but they do not currently
support API management functionality. Unlike a REST API, which receives and responds to requests, a **WebSocket
API** supports two-way communication between client apps and your backend. The backend can send callback messages to
connected clients.

## Designing WebSocket APIs

### Real-time message communication with WebSocket APIs

In a WebSocket API, the client and server can send messages to each other at any time. With a WebSocket connection, your
backend servers can **push data** to connected users and devices, avoiding the need to implement complex polling
mechanisms.

### Benefits and use cases of WebSocket APIs

WebSocket APIs are often used in real-time application use cases such as:

- Chat applications
- Streaming dashboards
- Real-time alerts and notifications
- Collaboration platforms
- Multiplayer games
- Financial trading platforms

### Pricing considerations for WebSocket APIs

With API Gateway WebSocket APIs, you only pay **when** your **APIs are in use**.

#### Flat charge

WebSocket APIs for API Gateway charge for the messages you send and receive. You can send and receive messages up to 128
KB in size. Messages are metered in 32-KB increments, so a 33-KB message is charged as two messages.

For WebSocket APIs, the API Gateway free tier currently includes one million messages (sent or received) and 750,000
connection minutes for up to 12 months.

#### Connection minutes

In addition to paying for the messages you send and receive, you are also charged for the total number of connection
minutes.

#### Additional charges

You may also incur additional charges if you use API Gateway in conjunction with other AWS services or transfer data out
of AWS.

## Developing a WebSocket API in API Gateway

### Creating and configuring WebSocket APIs

To create a functional API, you must have at least one route, integration, and stage before deploying the API.

![web-socket-api-details.png](images%2Fweb-socket-api-details.png)

Non-JSON messages are directed to a $default route.

**Route key** is the value expected once a route selection
expression is evaluated.

**Route selection expression** is an attribule defined at the API level.Specifies a JSON property expected to be present
in the message payload.

### Using WebSocket routes

With WebSocket APIs in API Gateway, JSON messages can be routed to invoke a specific backend service **based on message
content**. When a client sends a message over its WebSocket connection, this results in a route request to the WebSocket
API. The request will be matched to the route with the corresponding route key in API Gateway.

There are three predefined routes that can be used with WebSocket APIs: **$connect**, **$disconnect**, and **$default**.
In addition to the predefined routes, you can also create **custom** routes.

![web-socket-routes.png](images%2Fweb-socket-routes.png)

**Ways of using $default routing value:**

- with defined route keys to specify a fallback route
- without defined route keys to specify a proxy model that delegates routing to a backend component
- route for non-JSON payloads

After configuring the WebSocket API routes, whether predefined or custom, the next step is to attach integrations to
each route.

### WebSocket API integrations

After setting up an API route, you must integrate it with an endpoint in the backend. A backend endpoint is also
referred to as an **integration endpoint** and can be a **Lambda** function, an **HTTP** endpoint, or an **AWS
service** action. The API integration has an integration request and an integration response option.

#### Integration request

Setting up an integration request involves the following:

- Choosing a route key to integrate to the backend
- Specifying the backend endpoint to invoke for each of the routes, such as an AWS service or HTTP endpoint.
- Configuring how to transform the route request data, if necessary, into integration request data by specifying one or
  more request templates.

#### Integration response

WebSocket routes can be configured for two-way or one-way communication. If a route has a route response, it is
configured for two-way communication. Otherwise, it is configured for one-way communication.

- When a route is configured for **two-way communication**, an integration response helps you to configure
  transformations on the returned message payload, similar to integration responses for REST APIs.
- If a route is configured for **one-way communication** then, regardless of any integration response configuration, no
  response will be returned over the WebSocket channel after the message is processed.

### WebSocket selection expressions

WebSocket selection expressions

API Gateway uses **selection expressions** as a way to evaluate the request and response context and produce a key. This
key is then used to select from a set of possible values that you provide.

The selection expressions that you can use include:

- **Route response** selection expressions, which are used for modeling a response from the backend to the client
- **API key** selection expressions, which are evaluated when the service determines the given request should proceed
  only if the client provides a valid API key
- **API mapping** selection expressions, which are evaluated to determine which API stage is selected when a request is
  made using a custom domain

### Maintaining connections to WebSocket APIs

#### Connect

The client apps connect to your WebSocket API by sending a WebSocket upgrade request. If the request succeeds, the *
*$connect** route is invoked while the connection is being established. Until the invocation of the integration you
associated with the $connect route is completed, the upgrade request is pending and the actual connection will not be
established. If the **$connect** request fails, the connection will not be made.

#### Established connection

After the connection is established, your client's JSON messages can be routed to invoke a specific backend service
based on message content. When a client sends a message over its WebSocket connection, this results in a route request
to the WebSocket API. The request will be matched to the route with the corresponding route key in API Gateway.

#### Disconnect

The **$disconnect** route is invoked after the connection is closed. The connection can be closed by the server or by
the client. Since the connection is already closed when it is invoked, the **$disconnect** route is a best-effort event.
API Gateway will try its best to deliver the **$disconnect** event to your integration, but it cannot guarantee
delivery. The backend can initiate disconnection by using the **@connections** API.

## Designing REST APIs

### API Gateway REST API endpoint types

#### Regional endpoint

The regional endpoint is designed to reduce latency when calls are made from the same AWS Region as the API. In this
model, API Gateway does not deploy its own CloudFront distribution in front of your API. Instead, traffic destined for
your API will be directed straight at the API endpoint in the Region where you’ve deployed it.

This endpoint type gives you lower latency for applications that are invoking your API from within the same Region (for
example, an API that is going to be accessed from EC2 instances within the same Region).

The regional endpoint provides you with the flexibility to deploy your own CloudFront distribution or content delivery
network (CDN) in front of API Gateway and control that distribution using your own settings for customized scenarios. An
example of this might be to design for disaster recovery scenarios or implement load balancing in a very customized way.
Architecture with API Gateway highlighting regional traffic.

![regional-rest-endpoint.jpg](images%2Fregional-rest-endpoint.jpg)

#### Edge-optimized endpoint

The edge-optimized endpoint is designed to help you reduce client latency from anywhere on the internet. If you choose
an edge-optimized endpoint, API Gateway will automatically configure a fully managed CloudFront distribution to provide
lower latency access to your API.

This endpoint-type setup reduces your first hit latency for your API. An additional benefit of using a managed
CloudFront distribution is that you don’t have to pay for or manage a CDN separately from API Gateway.

![edge-optimized-endpoint.jpg](images%2Fedge-optimized-endpoint.jpg)

#### Private endpoint

The private endpoint is designed to expose APIs only inside your selected Amazon Virtual Private Cloud (Amazon VPC).
This endpoint type is still managed by API Gateway, but requests are only routable and can only originate from within a
single virtual private cloud (VPC) that you control.

This endpoint type is designed for applications that have very secure workloads, such as healthcare or financial data
that cannot be exposed publicly on the internet. There are no data transfer-out charges for private APIs. However, AWS
PrivateLink charges apply when using private APIs in API Gateway.

![private-endpoint.jpg](images%2Fprivate-endpoint.jpg)

The following **endpoint type changes** are supported:

1. From edge-optimized to regional or private
2. From regional to edge-optimized or private
3. From private to regional

> You cannot change a private API endpoint into an edge-optimized API endpoint.

### API Gateway optional cache

This is an optional configuration only available for REST APIs.

### Configure caching per API stage

Configuration choices for stage caching include the following.

#### Provision between 0.5 GB and 237 GB of cache

When you turn on caching, you can configure the size of the cache anywhere from half a gig to 237 gigabytes, and you can
also configure and customize the maximum TTL for each cache entry.

#### Set TTL in seconds

The default TTL value for API caching is 300 seconds. The maximum TTL value is 3,600 seconds. When you set TTL=0,
caching is turned off within API Gateway.

#### Turn on encryption of cache data

You can also encrypt the cached data if you need to.

#### Only GET methods will be cached

When you turn on caching in a stage's cache settings, only GET methods are cached. We recommend that you don’t cache
other types of calls unless you have very specific reasons.

#### Configure per method

You can override stage-level settings for individual methods. Turn caching on or off for specific methods, increase or
decrease the TTL, or turn encryption on or off for cached responses.

You can also use parameters in the method to form cache keys so that API Gateway caches the method's responses depending
on the parameter values used.

### Managing the API Gateway cache

#### Caching is charged at an hourly rate

Data caching is charged at an hourly rate that is dependent on the cache size you select, regardless of the number of
API calls being cached. So be thoughtful in choosing the cache size, and consider the amount of data you intend to
cache. Two ways to verify caching:

1) CloudWatch Metrics: CacheHitCount and CacheMissCount.
2) Create a timestamp and include it in your API response.

### Pricing considerations for REST APIs

#### Flat charge

REST APIs for API Gateway have a flat charge per million API Gateway requests. With API Gateway, you only pay when your
APIs are in use at a set cost per million requests.

The API Gateway free tier includes one million API calls per month for up to 12 months.

#### Data transfer out

An additional cost to factor into your cost estimates is the data transfer out of AWS that will be charged at standard
AWS prices. You may incur additional charges if you use API Gateway in conjunction with other AWS services or transfer
data out of AWS.

Private API endpoints don’t have data transfer-out charges. Instead, PrivateLink charges apply.

#### Optional cache

You can optionally provision a dedicated cache for each stage of your APIs. After you specify the size of the cache you
require, you will be charged an hourly rate for each stage's cache.

# Building and Deploying APIs with API Gateway

## Anatomy of the API call

### The base API invoke URL follows a pattern

When you deploy your API, you deploy to a stage, which will be discussed shortly. At that point, a base URL is generated
and displayed on the API stage editor. That base URI is called the **invoke URL**, and its composition will look like
this:
![invoke-url.jpg](images%2Finvoke-url.jpg)

### Customize the hostname

The domain name and the host can be customized. In addition, API Gateway is integrated with AWS Certificate Manager (
ACM) and lets you import your own certificate or generate a Secure Sockets Layer (SSL) certificate with ACM.

### Steps to build an API with API Gateway

#### 1. Choose an API type

To begin, under the API Gateway console, choose the **Create API** option. You will then need to choose which type of
API to build or import an API such as REST or WebSocket APIs.

![choose-api-type.png](images%2Fchoose-api-type.png)

#### 2. Create the API

After you have selected your API type, you will have the option to create a new API, clone an existing API, or import an
API as your starting point.
> When you save your API, AWS generates a unique ID for that API name, which becomes part of the hostname in the invoke
> URL.

#### 3. Add resources

#### 4. Configure resource as proxy

When adding a resource, you have the option to create a proxy resource as well. If you choose this option, it will
automatically create a special HTTP method called **ANY**.

A proxy resource is expressed by a special resource path parameter of **{proxy+}**, often referred to as a greedy path
parameter. The plus sign (+) indicates child resources appended to it.

In addition to the HTTP proxy, there is also a **Lambda proxy** option. With the Lambda proxy integration, the client
can call a single Lambda function in the backend. The function accesses many resources or features of other AWS
services, including calling other Lambda functions.

To use the proxy option, you first configure the resource as a proxy resource and then set up an integration type of
either HTTP or Lambda proxy when creating the method.

#### 5. Create method

### API Gateway integration types

#### Lambda function

When you are using API Gateway as the gateway to a Lambda function, you’ll use the Lambda integration. This will result
in requests being proxied to Lambda with request details available to your function handler in the event parameter,
supporting a streamlined integration setup. The setup can evolve with the backend without requiring you to tear down the
existing setup.

For integrations with Lambda functions, you will need to set an IAM role with required permissions for API Gateway to
call the backend on your behalf.

#### HTTP endpoint

HTTP integration endpoints are useful for public web applications where you want clients to interact with the endpoint.
This type of integration lets an API expose HTTP endpoints in the backend.

When the proxy is not configured, you’ll need to configure both the integration request and the integration response,
and set up necessary data mappings between the method request-response and the integration request-response.

If the proxy option is used, you don’t set the integration request or the integration response. API Gateway passes the
incoming request from the client to the HTTP endpoint and passes the outgoing response from the HTTP endpoint to the
client.

#### AWS service

AWS Service is an integration type that lets an API expose AWS service actions. For example, you might drop a message
directly into an Amazon Simple Queue Service (Amazon SQS) queue.

#### Mock

Mock lets API Gateway return a response without sending the request further to the backend. This is a good idea for a
health check endpoint to test your API. Anytime you want a hardcoded response to your API call, use a Mock integration.

#### VPC Link

With VPC Link, you can connect to a Network Load Balancer to get something in your private VPC. For example, consider an
endpoint on your EC2 instance that’s not public. API Gateway can’t access it unless you use the VPC link and you have to
have a Network Load Balancer on your backend.

For an API developer, a VPC Link is functionally equivalent to an integration endpoint.

#### 6. Edit method details

#### 7. Test API methods

### Response logs

Test results include simulated CloudWatch logs. No data is actually written to CloudWatch when testing.

## API stages

A stage is a snapshot of the API and represents a unique identifier for a version of a deployed API.

With stages, you can have multiple versions and roll back versions. Anytime you update anything about the API, you need
to redeploy it to an existing stage or to a new stage that you create as part of the deploy action.
The API Gateway URL with the word stage in the URL highlighted.

![api-stages.png](images%2Fapi-stages.png)

### Stage options

Critical design options are set per stage including:

- Caching
- Throttling
- Usage plans

At an API stage, you can also export the API definitions or generate an SDK for your users to call the API using a
supported programming language.

### Differentiate your APIs with stages

![api-stages1.png](images%2Fapi-stages1.png)

### Simplify version management with stage variables

As you define variables in the stage settings in the console, you can reference them with the *
*$stageVariables.[variable name]** notation. You can also inject stage-dependent items at runtime such as:

- URLs
- Lambda functions
- Any necessary variables

This is a great way to perform actions such as invoking different endpoints and different backends for different stages.
For example, if you have different endpoints with different URLs or different Lambda function aliases based on
environment, you can use stage variables to inject those variables.

### Stage variable example

![stage-variables.png](images%2Fstage-variables.png)

![stage-vars-usage.jpg](images%2Fstage-vars-usage.jpg)

Using stage variables is especially valuable as you move toward an automated continuous integration and continuous
delivery (CI/CD) pipeline.

## Building and deploying best practices

### Building and deploying best practices

Lambda and API Gateway are both designed to support flexible use of versions. You can do this by using aliases in Lambda
and stages in API Gateway. When you couple that with stage variables, you don't have to hard-code components, which
leads to having a smooth and safe deployment.

- In Lambda, enable **versioning** and use **aliases** to reference.
- In API Gateway, use **stages** for environments.
- Point API Gateway **stage variables** at the Lambda **aliases**.

### Use Canary deployments

With Canary deployments, you can send a percentage of traffic to your "canary" while leaving the bulk of your traffic on
a known good version of your API until the new version has been verified. API Gateway makes a base version available and
updated versions of the API on the same stage. This way, you can introduce new features in the same environment for the
base version.

To set up a Canary deployment through the console, select a stage and then select the Canary tab for that stage.

When you are sure the new method works, you can promote the canary and have all traffic flow through the new method.

### Use AWS SAM to simplify deployments

There are some best practices for deploying your APIs and serverless applications to production using AWS SAM as your
application framework.

#### Using SAM templates

AWS SAM provides templates that help you define your serverless applications. These template specifications provide you
with a straightforward and clean syntax to describe your functions, APIs, permissions, configurations, and events. You
use an AWS SAM template file to operate on a single, deployable, versioned entity that makes up your serverless
application.

AWS SAM is an extension of AWS CloudFormation, so it gives you the deployment capabilities and the full suite of
resources available in CloudFormation. The AWS SAM template file closely follows the format of a CloudFormation template
file.

#### Use Swagger and OpenAPI for more complex APIs

AWS SAM also supports OpenAPI to define more complex APIs. This can either be 2.0 for the Swagger specification, or one
of the OpenAPI 3.0 versions, like 3.0.1. OpenAPI is an industry-standard way to document and design your APIs.

With SAM, you can document your API in an external OpenAPI or Swagger file, and then reference that in a SAM template.

## Managing API Access

API Gateway provides you with multiple, customizable options for:

- Authorizing an entity to access your APIs
- Providing more granular control
- Controlling the amount of access through throttling

#### Authorization and authentication comparison

|                           | Authentication | Authorization | Signature V4 | Cognito User Pools | Third-Party Auth | Multiple Header Support | Additional Costs                       |
|---------------------------|----------------|---------------|--------------|--------------------|------------------|-------------------------|----------------------------------------|
| AWS IAM                   | +              | +             | +            |                    |                  |                         | None                                   |
| Lambda Authorizer Token   | +              | +             |              | +                  | +                |                         | Pay per authorizer invoke              |
| Lambda Authorizer Request | +              | +             |              | +                  | +                | +                       | Pay per authorizer invoke              |
| Amazon Cognito            | +              | +             |              | +                  |                  |                         | Pay based on your monthly active users |

### Authorization for API Gateway

There are three main ways to authorize API calls to your API Gateway endpoints:

1. Use IAM and **Signature version 4** (also known as Sig v4) to authenticate and authorize entities to access your
   APIs.
2. Use **Lambda Authorizers**, which you can use to support bearer token authentication strategies such as **OAuth** or
   **SAML**.
3. Use **Amazon Cognito** with **user pools**.

### Authorizing with IAM

If you have an **internal service** or a **restricted number of customers**, IAM is a great choice for authorization,
especially for applications that use IAM to interact with other AWS services using IAM roles.

![iam-auth.jpg](images%2Fiam-auth.jpg)

1) With IAM authorization, all requests are required to be signed using **Sig v4**
2) The process uses your AWS **access key** & **secret key** to compute an **HMAC signature** using **SHA256**. You can
   obtain these keys as an IAM user or by assuming an IAM role.
3) The key information is added to the **Authorization header**, and behind the scenes, API Gateway will take that
   signed request, parse it, and determine whether the user who signed the request has the IAM permissions to invoke
   your API.

   If not, API Gateway will simply deny and reject that request. So for this type of authentication, your **requestor
   must
   have AWS credentials**.

### Lambda Authorizers

![lambda-auth.jpg](images%2Flambda-auth.jpg)

1. A Lambda Authorizer is a Lambda function used to perform any custom authorization needed.
   Lambda **authorizer types**: **Token** & **Request**
2. When a client calls the API, API Gateway verifies whether th Lambda Authorizer is configured for the API method.

   If so, API Gateway calls the Lambda function
3. In this call, API Gateway supplies the authorization token (or the request parameters based on the authorizer), and
   the Lambda function returns the policy that allows or denies the caller's request
4. API Gateway also supports an optional policy cache that you can configure for your Lambda Authorizer.

   THis feature increases performance by reducing the number of invocations of your Lambda Authorizer for previously
   authorized tokens.ith this cache, you can configure a custom TTL.

### Lambda Authorizer token types

For token-type Lambda Authorizers, API Gateway passes the source token to the Lambda function as a JSON input. Based on
the value of this token, your Lambda function will determine whether to allow the request.

![lambda-token-auth.jpg](images%2Flambda-token-auth.jpg)

### Lambda Authorizer request types

Request-type Lambda Authorizers are useful if you need more information about the request itself before authorizing it.

![lambda-auth-request-type.jpg](images%2Flambda-auth-request-type.jpg)

### Cognito Authorizers

As an alternative to using IAM or Lambda authorizers, you can use Amazon Cognito and a Cognito User Pool to control
access to your APIs.

![cognito-auth.jpg](images%2Fcognito-auth.jpg)

![cognito-auth-1.png](images%2Fcognito-auth-1.png)

![cognito-auth-2.png](images%2Fcognito-auth-2.png)

![cognito-auth-3.png](images%2Fcognito-auth-3.png)

![cognito-auth-4.png](images%2Fcognito-auth-4.png)

## Throttling and usage plans

Beyond just allowing or denying access to your APIs, API Gateway also helps you manage the volume of API calls that are
processed through your API endpoint.

With API Gateway, you can set throttle and quota limits on your API consumers. This can useful for things such as
preventing one consumer from using all of your backend system’s capacity or to ensure that your downstream systems can
manage the number of requests you send through.

### API keys

With API Gateway, you can create and distribute API keys to your customers, which can be used to identify the consumer
and apply desired usage and throttle limits to their requests. Customers include the API key through x-API-key header in
requests.

### Usage plans

You can set throttle and quota limits based on API keys through the usage plans feature. You can set up usage plans for:

- API Key Throttling per second and burst
- API Key Quota by day, week, or month
- API Key Usage by daily usage records

### Example of usage plans based on types of consumers

![throttling.jpg](images%2Fthrottling.jpg)

With usage plans, you can create both the throttle rate limit and apply a daily quota.

### Token bucket algorithm

**The token bucket algorithm** is a widely used algorithm for checking that network traffic conforms to set limits. A
token, in this case, counts as a request and the burst is the maximum bucket size.

Requests that come into the bucket are fulfilled at a steady rate. If the rate at which the bucket is being filled
causes the bucket to fill up and exceed the burst value, a **429 Too Many Requests** error would be returned.

API Gateway sets a limit on a steady-state rate and a burst of request submissions per account and per Region. At the
account level, by default, API Gateway limits the steady-state request rate to 10,000 requests per second. It limits the
burst to 5,000 requests across all APIs within an AWS account.

![token-bucket.png](images%2Ftoken-bucket.png)

### Throttling settings hierarchy

The type and level of throttling applied to a request is dependent on all of the limits involved and are applied in this
order:

- **Per-client**, **per-method** throttling limits that you set for an API stage in a usage plan
- **Per-client** throttling limits that you set in a usage plan
- **Default per-method limits** and **individual per-method limits** that you set in API stage settings
- The **account level limit**

## IAM permissions

When it comes to granting access to your APIs, you need to think about two types of permissions:

1. Who can invoke the API: To call a deployed API, or refresh the API caching, the caller needs the **execute-api**
   permission.
2. Who can manage the API: To create, deploy, and manage an API in API Gateway, the API developer needs the
   **apigateway** permission.

### Invoke permissions

For the **execute-api** permission, you need to create IAM policies that permit a specified API caller to invoke the
desired API method. To apply this IAM policy on the API method, you need to configure the API method to use an
authorization type of **AWS_IAM**.

![execute-api.jpg](images%2Fexecute-api.jpg)

For the **execute-api** permission, create IAM policies that permit a specified API caller to invoke the desired API
method.

### Manage permissions

To allow an API developer to create and manage an API in API Gateway, you need IAM permission policies that allow a
specified API developer to create, update, deploy, view, or delete required API entities. To do that, create a policy
using the **apigateway:HTTP_VERB** format, associated with the specific resource using the verb that you want to permit
or deny in the policy.

![apigateway.jpg](images%2Fapigateway.jpg)

### Resource policies

****

## Monitoring and Troubleshooting

### CloudWatch Logs for API Gateway

**Execution logging** logs what’s happening on the roundtrip of a request.

Execution logs can be useful to troubleshoot APIs, but can result in **logging sensitive data**. Because of this, it is
recommended you don't enable Log full requests/responses data for production APIs. In addition, there is a cost
component associated with logging your APIs.

**Access logging** provides details about who's invoking your API. This includes everything including IP address, the
method used, the user protocol, and the agent that's invoking your API.

Access logging is fully customizable using JSON formatting.

## Monitoring with X-Ray and CloudTrail

### AWS X-Ray

You can use X-Ray to trace and analyze user requests as they travel through your Amazon API Gateway APIs to the
underlying services.

With X-Ray, you can trace and analyze requests as they travel through your APIs to services:

- Analyze latencies and debug errors in your APIs and their backend services.
- Configure sampling rules to focus on specific requests.

### AWS CloudTrail

The second service, CloudTrail, captures all API calls for API Gateway as events, including calls from the API Gateway
console and from code calls to your API Gateway APIs.

- IP address, requester, and time of request are included.
- Event history can be reviewed.
- Create a trail to send events to an Amazon Simple Storage Service (Amazon S3) bucket.

### X-Ray trace examples

This first example demonstrates API Gateway Latency metrics for a Lambda cold start.

![x-ray-trace-1.png](images%2Fx-ray-trace-1.png)
![x-ray-trace-2.png](images%2Fx-ray-trace-2.png)
![x-ray-trace-3.png](images%2Fx-ray-trace-3.png)
![x-ray-trace-4.png](images%2Fx-ray-trace-4.png)

Next, this example demonstrates the API Gateway Latency metrics for a Lambda warm start.

![x-ray-warm-1.png](images%2Fx-ray-warm-1.png)
![x-ray-warm-2.png](images%2Fx-ray-warm-2.png)
![x-ray-warm-3.png](images%2Fx-ray-warm-3.png)
![x-ray-warm-4.png](images%2Fx-ray-warm-4.png)

## Data Mapping and Request Validation

### Data transformations with mapping templates

![xml-to-json.png](images%2Fxml-to-json.png)

### Key variables for transformations

| Variable        | Use Case                                                                            |
|-----------------|-------------------------------------------------------------------------------------|
| $input          | Body, json, params, path                                                            |
| $stageVariables | Any stage variable name                                                             |
| $util           | escapeJavaScript()<br> parseJson()<br> urlEncode/Decode()<br> base64Encode/Decode() |

### Handling errors with Gateway Responses

For invalid requests, API Gateway bypasses the integration altogether and returns an error response. By default, the
error response contains a short descriptive error message.

For some of the error responses, API Gateway allows customization by API developers to return the responses in different
formats. You can access the Gateway Responses option on the API Gateway console. Some of the customization of various
error responses you can use are:

- Change HTTP status code.
- Modify body content.
- Add headers.

### Offloading request validation to API Gateway

In conjunction with data transformation and customized Gateway Responses, you can also let API Gateway handle some of
your basic validations, rather than making the call or building that validation into the backend. 

![request-validation.png](images%2Frequest-validation.png)
