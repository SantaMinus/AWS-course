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

**REST API**s are intended for APIs that require API proxy functionality and API management features in a single solution. **HTTP API**s are optimized for building APIs that proxy to Lambda functions or HTTP backends, making them ideal for serverless workloads. HTTP APIs are a cheaper and faster alternative to REST APIs, but they do not currently support API management functionality. Unlike a REST API, which receives and responds to requests, a **WebSocket
API** supports two-way communication between client apps and your backend. The backend can send callback messages to connected clients.

