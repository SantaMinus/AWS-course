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
