# 1. API Fundamentals — Complete learning roadmap

Here is the order I recommend following. It covers the concepts you'll need before moving into REST API development, Express, and FastAPI.

| No. | Topic                                       | Importance |
| --- | ------------------------------------------- | ---------- |
| 1   | What is an API?                             | Essential  |
| 2   | Why do we need APIs?                        | Essential  |
| 3   | How APIs work (client-server communication) | Essential  |
| 4   | Types of APIs                               | Important  |
| 5   | API vs REST API vs Web Services             | Essential  |
| 6   | Request and response structure              | Essential  |
| 7   | JSON and data exchange                      | Essential  |
| 8   | API endpoints and resources                 | Essential  |
| 9   | API contracts and documentation             | Important  |
| 10  | Synchronous vs asynchronous APIs            | Important  |
| 11  | Statelessness and scalability               | Important  |
| 12  | API versioning                              | Important  |
| 13  | Rate limiting and throttling                | Important  |
| 14  | API security fundamentals                   | Essential  |
| 15  | Common API design principles                | Essential  |

We'll cover these concepts one by one, with interview answers, practical examples, and how they apply when creating an API.

# 2. What is an API?

API stands for Application Programming Interface.

An API is a way for two software applications to communicate with each other using a defined set of rules.

For example, imagine you're building a React application that displays weather information.

React frontend

User requests weather

API

Communication interface

Backend / Weather service

Processes request and fetches data

Response returns through the API to React

For example, your React application sends a request:

JavaScript

```
fetch("https://api.example.com/weather?city=Pune")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

The server might return:

JSON

```
{
  "city": "Pune",
  "temperature": 30,
  "condition": "Sunny"
}
```

Your React application can then display the weather information.

Important: An API is not necessarily a backend server. It is the interface through which software communicates. The API may be provided by your own backend, an external service, a library, or even an operating system.

### Interview answer

Q: What is an API?

An API (Application Programming Interface) is a set of defined rules and protocols that allows different software applications to communicate and exchange data. In web development, APIs commonly use HTTP to allow frontend applications to interact with backend services.


# 3. Why do we need APIs?

Imagine you have built a React frontend and a Node.js backend for a student management application.

Your application needs to perform these operations:

* Register a student.

* Display all students.

* Update student information.

* Delete a student.

How does React communicate with Node.js to perform these operations?

This is where APIs are needed.

### Example: Student management application

React frontend

User clicks "Get Students"

GET /api/students

HTTP request

Express backend

Validates request, executes logic, queries database

MongoDB

Returns student records

JSON response

Express sends data back to React

### Main reasons for using APIs

| Reason                 | Explanation                                                                         |
| ---------------------- | ----------------------------------------------------------------------------------- |
| Communication          | Allows frontend and backend to exchange data                                        |
| Separation of concerns | Frontend handles UI; backend handles business logic and data access                 |
| Reusability            | Multiple clients can use the same backend API                                       |
| Security               | Sensitive operations and database access can stay on the server                     |
| Integration            | Connects applications with external services, such as payment gateways or AI models |
| Scalability            | Frontend and backend can be developed and scaled independently                      |

For example, your React frontend and a mobile app could both use the same Express backend API.

### Interview question

Q: Why do we use APIs instead of directly connecting React to MongoDB?

Answer: APIs provide a controlled interface between the frontend and the database layer. The backend handles authentication, authorization, validation, and business logic. This prevents exposing database credentials and allows us to control which data and operations clients can access.

# 4. How does API communication work?

This is one of the most important fundamentals for interviews and real-world API development.

Let's understand the flow step by step.

1. Client initiates a request

   A client, such as React, sends an HTTP request to a backend endpoint.

   http

   ```
   GET /api/students HTTP/1.1
   Host: example.com
   Accept: application/json
   ```

2. Server receives and processes the request

   Express matches the URL and HTTP method to a route, then runs middleware and controller/business logic as needed.

   JavaScript

   ```
   app.get("/api/students", async (req, res) => {
     const students = await Student.find();
     res.status(200).json(students);
   });
   ```

3. Backend communicates with the database

   The backend fetches the required student records from MongoDB.

4. Server sends a response

   http

   ```
   HTTP/1.1 200 OK
   Content-Type: application/json
   ```

   JSON

   ```
   [
     { "id": 1, "name": "Vibha" },
     { "id": 2, "name": "Amit" }
   ]
   ```

5. Frontend handles the response

   React receives the response, parses the JSON and updates its state to render the data.

Notice that there are two different communications here:

* React to Express: HTTP request and response.

* Express to MongoDB: database query and result.

The API is the interface exposed by Express. The database query is internal backend work, not an HTTP API request in this example.


# 5. Types of APIs

You may be asked about API types in interviews. There are different ways to classify APIs.

## Based on access

| Type          | Meaning                                                                      | Example                                                     |
| ------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------- |
| Public API    | Available for external developers, sometimes with registration or an API key | OpenWeather API                                             |
| Private API   | Used internally within an organization                                       | Internal employee management API                            |
| Partner API   | Shared with selected business partners                                       | Payment provider integration                                |
| Composite API | Combines multiple API calls or services into one operation                   | An API that returns user details and order history together |

## Based on architectural style or protocol

| Type          | Description                                                      | Common use                         |
| ------------- | ---------------------------------------------------------------- | ---------------------------------- |
| REST          | Uses HTTP resources and methods                                  | MERN applications                  |
| SOAP          | Structured messaging protocol, usually XML-based                 | Enterprise systems                 |
| GraphQL       | Clients request specific fields using a query language           | Complex frontend data requirements |
| gRPC          | High-performance RPC framework using Protocol Buffers by default | Microservices communication        |
| WebSocket API | Supports two-way communication over a persistent connection      | Real-time chat applications        |

Interview question: What is the difference between REST and GraphQL?

* REST usually exposes multiple resource-oriented endpoints.

* GraphQL typically exposes a single endpoint where clients specify which data they need.

* REST commonly uses HTTP methods to express operations, while GraphQL uses queries, mutations, and optionally subscriptions.

For example, your chat application uses Socket.IO for real-time communication. This is different from the typical request-response flow of a REST API.

# 6. What is the difference between API, REST API and Web Service?

This is a common interview question because these terms are often used interchangeably, even though they aren't identical.

| Term        | Meaning                                                                                |
| ----------- | -------------------------------------------------------------------------------------- |
| API         | A general interface that allows software components to interact                        |
| Web API     | An API accessible over web technologies, often HTTP                                    |
| REST API    | A Web API designed according to REST architectural constraints                         |
| Web service | A software system that exposes functionality over a network, often using web protocols |

Every REST API is a type of API. However, not every API is a REST API, and not every API is necessarily a web service.

Interview answer:

A REST API is an API that follows REST architectural constraints, such as stateless communication, a uniform interface, and resource-based interactions. An API is a broader concept and can also refer to interfaces such as browser APIs, library APIs, and operating system APIs.

# 7. What should you understand before creating an API?

These are the practical decisions you should learn to make when developing an API, regardless of whether you're using Express or FastAPI.

## API creation checklist

Your learning checklist

0/10 completed

Identify the client and the problem the API will solve

Define the resources, such as users, products or predictions

Design endpoints and HTTP methods

Determine request parameters, headers and body

Define the response structure and status codes

Add validation and error handling

Decide authentication and authorization requirements

Consider security, CORS and rate limits

Document the API contract

Test normal cases and failure cases

You don't need to master every item before writing your first API. The goal is to understand the purpose of each decision and learn how to implement it.

## Quick interview revision

Try answering before revealing the answers.

1. What does API stand for?

Application Programming Interface. It defines how software components interact.

Hide answer

2. Is an API the same as a backend?

No. The API is an interface. A backend is a system that may expose APIs and perform business logic.

Hide answer

3. Can multiple applications use the same API?

Yes, provided they have the necessary access and follow the API's contract.

Hide answer

4. What is a client-server architecture?

A model in which a client sends requests and a server processes them and returns responses.

Hide answer

5. What is the most common data format used by REST APIs?

JSON is very common, although REST APIs can support other representations too.

Hide answer

