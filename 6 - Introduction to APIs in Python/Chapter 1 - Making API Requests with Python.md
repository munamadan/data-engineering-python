## What is an API?

**API** = **Application Programming Interface**

- A set of communication rules and abilities
- Enables interactions between software applications
- Defines how different software components should communicate

## Web APIs: Client-Server Architecture

### Basic Concept

Web APIs communicate over the internet using **HTTP** (HyperText Transfer Protocol)

### Request/Response Cycle

1. **Client** sends a request message to a **Server**
2. **Server** processes the request
3. **Server** returns a response message to the **Client**

> [!info] Key Terms
> 
> - **Client**: The application making the request (your Python script)
> - **Server**: The system hosting the API and providing the data/service
> - **HTTP**: The protocol used for communication over the web

## Types of Web APIs

|API Type|Focus|Use Case|
|---|---|---|
|**SOAP**|Strict and formal design|Enterprise applications|
|**REST**|Simplicity & scalability|Most common API architecture|
|**GraphQL**|Flexibility & performance|Optimized data fetching|

**REST** (Representational State Transfer) is the most commonly used API architecture.

---

## Working with APIs in Python

### Two Main Libraries

#### 1. urllib (Built-in)

- Bundled with Python (no installation needed)
- Powerful but not very developer-friendly
- More verbose syntax

```python
from urllib.request import urlopen

api = "http://api.music-catalog.com/"
with urlopen(api) as response:
    data = response.read()
    string = data.decode()
    print(string)
```

#### 2. requests (Recommended)

- Many powerful built-in features
- Easier to use and more intuitive
- Industry standard for API requests

```python
import requests

api = "http://api.music-catalog.com/"
response = requests.get(api)
print(response.text)
```

> [!tip] Best Practice Use the `requests` library for API work in Python - it's simpler and more feature-rich.

---

## Anatomy of a URL

**URL** = **Uniform Resource Locator**

The structured address to an API resource that allows you to interact with specific endpoints.

### URL Components

```
http://350.5th-ave.com:80/unit/243?floor=77&elevator=True
└──┘   └────────────────┘ └┘ └──────┘ └──────────────────────┘
  │            │          │      │              │
Protocol    Domain      Port   Path         Query
```

|Component|Description|Example|
|---|---|---|
|**Protocol**|Means of transportation (http/https)|`http://`|
|**Domain**|Street address of the server|`350.5th-ave.com`|
|**Port**|Gate/door to use (often omitted)|`:80`|
|**Path**|Specific resource/endpoint|`/unit/243`|
|**Query**|Additional instructions/parameters|`?floor=77&elevator=True`|

---

## Query Parameters

### Method 1: Append to URL String

```python
response = requests.get('http://350.5th-ave.com/unit/243?floor=77&elevator=True')
print(response.url)
# Output: http://350.5th-ave.com/unit/243?floor=77&elevator=True
```

### Method 2: Use params Argument (Recommended)

```python
# Create dictionary
query_params = {'floor': 77, 'elevator': True}

# Pass the dictionary using the `params` argument
response = requests.get('http://350.5th-ave.com/unit/243', params=query_params)
print(response.url)
# Output: http://350.5th-ave.com/unit/243?floor=77&elevator=True
```

> [!tip] Best Practice Use the `params` argument with a dictionary - it's cleaner and handles URL encoding automatically.

---

## HTTP Verbs (Methods)

The **action** you want to perform on a resource.

|Verb|Action|Description|Analogy|
|---|---|---|---|
|**GET**|Read|Retrieve a resource|Check mailbox contents|
|**POST**|Create|Create a new resource|Drop new package in mailbox|
|**PUT**|Update|Replace/update existing resource|Replace all packages|
|**DELETE**|Delete|Remove a resource|Remove all packages|

> [!info] Note There are 9 HTTP verbs total, but these 4 are the most relevant for simple REST APIs.

### Using HTTP Verbs with requests

```python
# GET = Retrieve a resource
response = requests.get('http://350.5th-ave.com/unit/243')

# POST = Create a resource
response = requests.post('http://350.5th-ave.com/unit/243', data={"key": "value"})

# PUT = Update an existing resource
response = requests.put('http://350.5th-ave.com/unit/243', data={"key": "value"})

# DELETE = Remove a resource
response = requests.delete('http://350.5th-ave.com/unit/243')
```

> [!important] Key Point Each verb has its own method in the requests package. Use the `data` argument to pass data to POST or PUT requests.

---

## HTTP Status Codes

Status codes indicate the outcome of an HTTP request.

### Status Code Categories

|Category|Range|Meaning|
|---|---|---|
|**1XX**|100-199|Informational responses|
|**2XX**|200-299|Successful responses|
|**3XX**|300-399|Redirection messages|
|**4XX**|400-499|Client error responses|
|**5XX**|500-599|Server error responses|

### Frequently Used Status Codes

|Code|Name|Meaning|
|---|---|---|
|**200**|OK|Request successful|
|**404**|Not Found|Resource doesn't exist|
|**500**|Internal Server Error|Server encountered an error|

### Accessing Status Codes in requests

```python
# Accessing the status code
response = requests.get('https://api.datacamp.com/users/12')
response.status_code == 200  # True

# Using requests.codes for readability
response = requests.get('https://api.datacamp.com/this/is/the/wrong/path')
response.status_code == requests.codes.not_found  # True
```

---

## HTTP Headers

Headers contain **metadata** about the request or response.

### Structure

```
key1: Value 1
key2: Value 2
```

### Common Use Case: Content Negotiation

**Client Request:**

```
Accept: application/json
```

**Server Response:**

```
Content-Type: application/json
```

This tells the server what format you want (JSON) and the server confirms what format it's sending back.

### Working with Headers in requests

#### Adding Headers to a Request

```python
response = requests.get(
    'https://api.datacamp.com',
    headers={'accept': 'application/json'}
)
```

#### Reading Response Headers

```python
# Dictionary-style access
response.headers['content-type']  # 'application/json'

# Using .get() method (safer - won't error if key missing)
response.headers.get('content-type')  # 'application/json'
```

---

## Request and Response Message Anatomy

### Request Message Structure

```
[Start-line: HTTP Method + URL + HTTP Version]
[Headers: key-value pairs]
[Empty line]
[Body: optional data]
```

### Response Message Structure

```
[Start-line: HTTP Version + Status Code + Status Text]
[Headers: key-value pairs]
[Empty line]
[Body: response data]
```

> [!info] The Start-Line A server will **always** include a numeric status code in the response message's start-line.

---

## Quick Reference: requests Library Methods

```python
import requests

# Basic GET request
response = requests.get('https://api.example.com/endpoint')

# GET with query parameters
response = requests.get('https://api.example.com/endpoint', 
                       params={'key': 'value'})

# POST with data
response = requests.post('https://api.example.com/endpoint', 
                        data={'key': 'value'})

# GET with custom headers
response = requests.get('https://api.example.com/endpoint',
                       headers={'Authorization': 'Bearer token123'})

# Access response attributes
print(response.status_code)  # Status code (e.g., 200)
print(response.text)          # Response body as string
print(response.headers)       # Response headers
print(response.url)           # Final URL (after redirects)
```

---

## Key Takeaways

✅ APIs enable communication between software applications  
✅ Web APIs use HTTP for client-server communication  
✅ REST is the most common API architecture  
✅ Use the `requests` library in Python (not urllib)  
✅ URLs contain: protocol, domain, port, path, and query parameters  
✅ Four main HTTP verbs: GET, POST, PUT, DELETE  
✅ Status codes indicate success (2XX) or errors (4XX, 5XX)  
✅ Headers contain metadata for content negotiation and more

---

#datacamp #apis #python #rest #http #certification

## Section A: Multiple Choice Questions (60 points)

_2 points each | 30 questions total_

### Topic 1: API Fundamentals & Architecture (Questions 1-10)

**Q1.** What does API stand for?

- [ ] A) Application Process Interface
- [ ] B) Application Programming Interface
- [ ] C) Automated Programming Interface
- [ ] D) Application Protocol Interface

> [!success]- Answer **Correct Answer: B) Application Programming Interface**
> 
> API stands for Application Programming Interface. It is a set of communication rules and abilities that enables interactions between software applications.

**Q2.** In the client-server model for Web APIs, what does the client do?

- [ ] A) Hosts the API and provides data
- [ ] B) Processes requests and returns responses
- [ ] C) Sends a request message to the server
- [ ] D) Stores data in a database

> [!success]- Answer **Correct Answer: C) Sends a request message to the server**
> 
> The client is the application making the request. It sends a request message to the server, which then processes it and returns a response. The server is responsible for hosting the API and providing data.

**Q3.** Which type of Web API focuses on simplicity and scalability and is the most common API architecture?

- [ ] A) SOAP
- [ ] B) GraphQL
- [ ] C) REST
- [ ] D) XML-RPC

> [!success]- Answer **Correct Answer: C) REST**
> 
> REST (Representational State Transfer) focuses on simplicity and scalability and is the most common API architecture. SOAP focuses on strict formal design for enterprise applications, while GraphQL focuses on flexibility and performance optimization.

**Q4.** What protocol do Web APIs use to communicate over the internet?

- [ ] A) FTP
- [ ] B) SMTP
- [ ] C) HTTP
- [ ] D) SSH

> [!success]- Answer **Correct Answer: C) HTTP**
> 
> Web APIs communicate over the internet using HTTP (HyperText Transfer Protocol). This is the foundation of data communication on the World Wide Web.

**Q5.** Which Python library for working with APIs is bundled with Python but is considered less developer-friendly?

- [ ] A) requests
- [ ] B) urllib
- [ ] C) httplib
- [ ] D) api-client

> [!success]- Answer **Correct Answer: B) urllib**
> 
> urllib is bundled with Python and is powerful, but it's not very developer-friendly. It requires more verbose syntax compared to the requests library, which is easier to use and has many built-in features.

**Q6.** What is the recommended Python library for making API requests due to its ease of use?

- [ ] A) urllib
- [ ] B) http.client
- [ ] C) requests
- [ ] D) api-wrapper

> [!success]- Answer **Correct Answer: C) requests**
> 
> The requests library is recommended because it has many powerful built-in features and is easier to use than urllib. It's the industry standard for making HTTP requests in Python.

**Q7.** Which of the following best describes what a URL is?

- [ ] A) Universal Resource Loader
- [ ] B) Uniform Resource Locator - the structured address to an API resource
- [ ] C) Unified Response Language
- [ ] D) User Request Locator

> [!success]- Answer **Correct Answer: B) Uniform Resource Locator - the structured address to an API resource**
> 
> URL stands for Uniform Resource Locator. It's the structured address that points to an API resource and allows you to interact with specific endpoints.

**Q8.** In the URL `http://api.example.com:8080/users/123?active=true`, what is `http://` called?

- [ ] A) Domain
- [ ] B) Protocol
- [ ] C) Path
- [ ] D) Query parameter

> [!success]- Answer **Correct Answer: B) Protocol**
> 
> The protocol (http:// or https://) is the means of transportation - it defines how data is transmitted between the client and server. In this example, HTTP is being used.

**Q9.** What is SOAP primarily used for?

- [ ] A) Simple web applications
- [ ] B) Mobile apps requiring flexibility
- [ ] C) Enterprise applications with strict formal API design
- [ ] D) Performance-optimized data fetching

> [!success]- Answer **Correct Answer: C) Enterprise applications with strict formal API design**
> 
> SOAP (Simple Object Access Protocol) focuses on strict and formal API design, making it suitable for enterprise applications that require rigid standards and contracts.

**Q10.** What completes the request/response cycle in Web APIs?

- [ ] A) Client sends request → Server processes → Server returns response
- [ ] B) Server sends request → Client processes → Client returns response
- [ ] C) Client and Server exchange data simultaneously
- [ ] D) Database queries the server directly

> [!success]- Answer **Correct Answer: A) Client sends request → Server processes → Server returns response**
> 
> The request/response cycle follows this order: the client initiates by sending a request to the server, the server processes that request, and then the server returns a response back to the client.

### Topic 2: URLs, Query Parameters & HTTP Verbs (Questions 11-20)

**Q11.** In the URL `https://api.store.com:443/products/phones?brand=apple&color=black`, what is `/products/phones`?

- [ ] A) Protocol
- [ ] B) Domain
- [ ] C) Path
- [ ] D) Query parameters

> [!success]- Answer **Correct Answer: C) Path**
> 
> The path `/products/phones` is the specific resource or endpoint within the API. It's like the specific office unit inside a building - it tells the server exactly which resource you want to access.

**Q12.** What is the recommended way to add query parameters to a request using the requests library?

- [ ] A) Append them directly to the URL string
- [ ] B) Use the `params` argument with a dictionary
- [ ] C) Add them in the request body
- [ ] D) Use the `query` argument

> [!success]- Answer **Correct Answer: B) Use the `params` argument with a dictionary**
> 
> Using the `params` argument with a dictionary is the recommended approach because it's cleaner, more readable, and automatically handles URL encoding. Example:
> 
> ```python
> query_params = {'floor': 77, 'elevator': True}
> response = requests.get('http://api.example.com', params=query_params)
> ```

**Q13.** Which HTTP verb is used to retrieve/read a resource?

- [ ] A) POST
- [ ] B) PUT
- [ ] C) GET
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: C) GET**
> 
> GET is used to retrieve or read a resource from the server. It's like checking the contents of a mailbox - you're just looking, not modifying anything.

**Q14.** Which HTTP verb would you use to create a new resource?

- [ ] A) GET
- [ ] B) POST
- [ ] C) PUT
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: B) POST**
> 
> POST is used to create a new resource on the server. It's like dropping a new package in the mailbox. You typically send data in the request body when using POST.

**Q15.** What argument do you use in the requests library to send data with a POST request?

- [ ] A) params
- [ ] B) body
- [ ] C) data
- [ ] D) payload

> [!success]- Answer **Correct Answer: C) data**
> 
> Use the `data` argument to pass data to a POST or PUT request:
> 
> ```python
> response = requests.post('http://api.example.com', data={"key": "value"})
> ```

**Q16.** Which HTTP verb replaces or updates an existing resource?

- [ ] A) GET
- [ ] B) POST
- [ ] C) PUT
- [ ] D) PATCH

> [!success]- Answer **Correct Answer: C) PUT**
> 
> PUT is used to update or replace an existing resource. It's like replacing all packages in a mailbox with a new one. The entire resource is typically replaced with the new data.

**Q17.** How many total HTTP verbs exist?

- [ ] A) 4
- [ ] B) 6
- [ ] C) 9
- [ ] D) 12

> [!success]- Answer **Correct Answer: C) 9**
> 
> There are 9 HTTP verbs in total, but for simple REST APIs only 4 are relevant: GET, POST, PUT, and DELETE. The others include HEAD, OPTIONS, PATCH, CONNECT, and TRACE.

**Q18.** In a URL, what separates the path from the query parameters?

- [ ] A) Ampersand (&)
- [ ] B) Question mark (?)
- [ ] C) Forward slash (/)
- [ ] D) Colon (:)

> [!success]- Answer **Correct Answer: B) Question mark (?)**
> 
> The question mark (?) separates the path from the query parameters in a URL. Example: `http://api.com/users?id=123` - everything after the ? is a query parameter.

**Q19.** What does the domain component of a URL represent?

- [ ] A) The means of transportation
- [ ] B) The street address of the server
- [ ] C) The specific resource
- [ ] D) Additional instructions

> [!success]- Answer **Correct Answer: B) The street address of the server**
> 
> The domain (like `api.example.com`) is the street address of the office building - it identifies which server you're trying to reach on the internet.

**Q20.** Which HTTP verb removes a resource?

- [ ] A) GET
- [ ] B) POST
- [ ] C) PUT
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: D) DELETE**
> 
> DELETE is used to remove a resource from the server. It's like removing all packages from the mailbox. Example:
> 
> ```python
> response = requests.delete('http://api.example.com/resource/123')
> ```

### Topic 3: Headers, Status Codes & Message Anatomy (Questions 21-30)

**Q21.** What HTTP status code indicates a successful request?

- [ ] A) 100
- [ ] B) 200
- [ ] C) 404
- [ ] D) 500

> [!success]- Answer **Correct Answer: B) 200**
> 
> Status code 200 means "OK" - the request was successful. It's part of the 2XX category which represents successful responses.

**Q22.** What does the HTTP status code 404 mean?

- [ ] A) Internal Server Error
- [ ] B) Not Found
- [ ] C) Unauthorized
- [ ] D) Bad Request

> [!success]- Answer **Correct Answer: B) Not Found**
> 
> 404 means "Not Found" - the requested resource doesn't exist on the server. It's a client error (4XX category) indicating the URL or resource couldn't be located.

**Q23.** Status codes in the 5XX range indicate:

- [ ] A) Successful responses
- [ ] B) Redirection messages
- [ ] C) Client error responses
- [ ] D) Server error responses

> [!success]- Answer **Correct Answer: D) Server error responses**
> 
> 5XX status codes indicate server errors - something went wrong on the server side while processing the request. For example, 500 is "Internal Server Error".

**Q24.** What HTTP header is commonly used by the client to specify the desired response format?

- [ ] A) Content-Type
- [ ] B) Accept
- [ ] C) Authorization
- [ ] D) User-Agent

> [!success]- Answer **Correct Answer: B) Accept**
> 
> The `Accept` header tells the server what format the client wants to receive (like `application/json`). This is part of content negotiation. The server then responds with a `Content-Type` header indicating what format it's sending.

**Q25.** How do you access the status code of a response in the requests library?

- [ ] A) response.code
- [ ] B) response.status
- [ ] C) response.status_code
- [ ] D) response.http_code

> [!success]- Answer **Correct Answer: C) response.status_code**
> 
> Use the `status_code` attribute:
> 
> ```python
> response = requests.get('https://api.example.com')
> if response.status_code == 200:
>     print("Success!")
> ```

**Q26.** Which method is safer for reading response headers that might not exist?

- [ ] A) response.headers['key']
- [ ] B) response.headers.get('key')
- [ ] C) response.headers.find('key')
- [ ] D) response.headers.read('key')

> [!success]- Answer **Correct Answer: B) response.headers.get('key')**
> 
> The `.get()` method is safer because it returns `None` if the key doesn't exist, whereas dictionary-style access (`response.headers['key']`) will raise a KeyError if the key is missing.

**Q27.** Status codes in the 4XX range indicate:

- [ ] A) Successful responses
- [ ] B) Server error responses
- [ ] C) Client error responses
- [ ] D) Informational responses

> [!success]- Answer **Correct Answer: C) Client error responses**
> 
> 4XX status codes indicate client errors - the request from the client had some problem (like 404 Not Found, 400 Bad Request, 401 Unauthorized, etc.).

**Q28.** What does the HTTP header `Content-Type` specify?

- [ ] A) The format the client wants to receive
- [ ] B) The format the server is sending
- [ ] C) The authentication credentials
- [ ] D) The API version

> [!success]- Answer **Correct Answer: B) The format the server is sending**
> 
> The `Content-Type` header in the response tells the client what format the server is sending (like `application/json`, `text/html`, etc.). This is the server's side of content negotiation.

**Q29.** What is always included in a server's response message start-line?

- [ ] A) The request URL
- [ ] B) A numeric status code
- [ ] C) The request body
- [ ] D) Query parameters

> [!success]- Answer **Correct Answer: B) A numeric status code**
> 
> A server will always include a numeric status code in the response message's start-line. The start-line format is: HTTP Version + Status Code + Status Text (e.g., "HTTP/1.1 200 OK").

**Q30.** How do you add custom headers to a request using the requests library?

- [ ] A) Using the `params` argument
- [ ] B) Using the `data` argument
- [ ] C) Using the `headers` argument
- [ ] D) Using the `metadata` argument

> [!success]- Answer **Correct Answer: C) Using the `headers` argument**
> 
> Pass a dictionary to the `headers` argument:
> 
> ```python
> response = requests.get(
>     'https://api.example.com',
>     headers={'accept': 'application/json', 'Authorization': 'Bearer token'}
> )
> ```

---

## Section B: True/False Questions (15 points)

_1 point each | 15 questions total_

**Q31.** The requests library is bundled with Python and doesn't need to be installed separately. **T/F**

> [!success]- Answer **False** - The requests library is NOT bundled with Python and must be installed separately (e.g., `pip install requests`). urllib is the library that comes bundled with Python.

**Q32.** REST is the most common API architecture. **T/F**

> [!success]- Answer **True** - REST (Representational State Transfer) focuses on simplicity and scalability and is indeed the most common API architecture used today.

**Q33.** The GET HTTP verb is used to create new resources on the server. **T/F**

> [!success]- Answer **False** - GET is used to read/retrieve resources, not create them. POST is the HTTP verb used to create new resources.

**Q34.** Status code 500 indicates a successful response. **T/F**

> [!success]- Answer **False** - Status code 500 means "Internal Server Error" and indicates a server-side error (5XX category). Status code 200 indicates success.

**Q35.** Query parameters are separated from the path in a URL by a question mark (?). **T/F**

> [!success]- Answer **True** - The question mark separates the path from query parameters. Example: `http://api.com/users?id=123&name=john`

**Q36.** The Accept header is used by the server to tell the client what format it's sending. **T/F**

> [!success]- Answer **False** - The Accept header is used by the CLIENT to tell the server what format it wants to receive. The server uses Content-Type to tell the client what format it's sending.

**Q37.** There are exactly 4 HTTP verbs in total. **T/F**

> [!success]- Answer **False** - There are 9 HTTP verbs in total, but only 4 (GET, POST, PUT, DELETE) are commonly used for simple REST APIs.

**Q38.** The params argument in requests.get() is used to add query parameters. **T/F**

> [!success]- Answer **True** - The `params` argument accepts a dictionary and automatically adds query parameters to the URL with proper encoding.

**Q39.** Status codes in the 2XX range indicate successful responses. **T/F**

> [!success]- Answer **True** - The 2XX category (200-299) represents successful responses, with 200 being the most common (OK).

**Q40.** The DELETE HTTP verb is used to update existing resources. **T/F**

> [!success]- Answer **False** - DELETE is used to remove resources. PUT is used to update/replace existing resources.

**Q41.** urllib is easier to use than requests for making API calls. **T/F**

> [!success]- Answer **False** - urllib is powerful but not very developer-friendly. The requests library is easier to use and has more built-in features.

**Q42.** GraphQL focuses on flexibility and performance optimization. **T/F**

> [!success]- Answer **True** - GraphQL is designed to focus on flexibility and is optimized for performance, allowing clients to request exactly the data they need.

**Q43.** The response.headers.get() method will raise an error if the header key doesn't exist. **T/F**

> [!success]- Answer **False** - The `.get()` method returns None if the key doesn't exist, making it safer than dictionary-style access which would raise a KeyError.

**Q44.** In Web APIs, the server sends a request message to the client. **T/F**

> [!success]- Answer **False** - The client sends the request message to the server. The server then returns a response message to the client.

**Q45.** The data argument is used in requests.post() to send data in the request body. **T/F**

> [!success]- Answer **True** - The `data` argument is used to pass data to POST and PUT requests, which is sent in the request body.

---

## Section C: Short Answer Questions (30 points)

_3 points each | 10 questions total_

**Q46.** Explain the difference between urllib and requests libraries. Why is requests generally preferred?

> [!success]- Answer **urllib** is bundled with Python and is powerful but not very developer-friendly - it requires more verbose syntax. **requests** has many powerful built-in features and is much easier to use, requiring less code to accomplish the same tasks.
> 
> **Requests is preferred because:**
> 
> - Cleaner, more intuitive syntax
> - Better handling of parameters, headers, and data
> - Automatic URL encoding
> - Better error handling
> - More Pythonic API design
> - Industry standard for HTTP requests in Python
> 
> **Example comparison:**
> 
> ```python
> # urllib - verbose
> from urllib.request import urlopen
> with urlopen(api) as response:
>     data = response.read()
>     string = data.decode()
> 
> # requests - simple
> import requests
> response = requests.get(api)
> data = response.text
> ```

**Q47.** List and describe the five components of a URL with an example.

> [!success]- Answer Using example: `https://api.store.com:443/products?category=electronics`
> 
> 1. **Protocol** (`https://`) - The means of transportation; defines how data is transmitted (HTTP or HTTPS)
>     
> 2. **Domain** (`api.store.com`) - The street address of the server; identifies which server to reach on the internet
>     
> 3. **Port** (`:443`) - The gate/door to use; specifies which port to connect to (often omitted as defaults are used: 80 for HTTP, 443 for HTTPS)
>     
> 4. **Path** (`/products`) - The specific office unit/resource; tells the server exactly which endpoint you want to access
>     
> 5. **Query** (`?category=electronics`) - Additional instructions; parameters that filter or modify the request
>     

**Q48.** Describe the request/response cycle in Web APIs and explain the role of HTTP.

> [!success]- Answer **The Request/Response Cycle:**
> 
> 1. **Client sends request**: The client (your Python script) initiates communication by sending an HTTP request message to the server
>     
> 2. **Server processes**: The server receives the request, processes it, and prepares the appropriate data/response
>     
> 3. **Server returns response**: The server sends an HTTP response message back to the client with the requested data or an error message
>     
> 
> **Role of HTTP:** HTTP (HyperText Transfer Protocol) is the protocol that enables this communication over the internet. It defines:
> 
> - How messages are formatted
> - How requests and responses are structured
> - What methods (verbs) can be used
> - How data is transferred between client and server
> - Status codes to indicate outcomes
> 
> HTTP acts as the language/rules that both client and server understand for communication.

**Q49.** Compare and contrast the four main HTTP verbs (GET, POST, PUT, DELETE) with examples of when to use each.

> [!success]- Answer
> 
> |Verb|Action|When to Use|Example|
> |---|---|---|---|
> |**GET**|Read/Retrieve|Fetching data without modifying it|Get list of users, retrieve product details|
> |**POST**|Create|Creating new resources|Register new user, create new blog post|
> |**PUT**|Update/Replace|Updating/replacing existing resources|Update user profile, replace product info|
> |**DELETE**|Remove|Deleting resources|Delete user account, remove product|
> 
> **Code examples:**
> 
> ```python
> # GET - Retrieve users
> response = requests.get('https://api.example.com/users')
> 
> # POST - Create new user
> response = requests.post('https://api.example.com/users', 
>                          data={'name': 'John', 'email': 'john@example.com'})
> 
> # PUT - Update existing user
> response = requests.put('https://api.example.com/users/123', 
>                         data={'name': 'John Doe'})
> 
> # DELETE - Remove user
> response = requests.delete('https://api.example.com/users/123')
> ```
> 
> **Key differences:**
> 
> - GET and DELETE don't typically include a body
> - POST and PUT use the `data` argument to send information
> - GET is idempotent (repeated calls have same effect)
> - POST creates new resources each time

**Q50.** Explain what HTTP headers are and describe the concept of content negotiation between client and server.

> [!success]- Answer **HTTP Headers:** Headers are key-value pairs that contain metadata about the request or response. They provide additional information beyond the URL and body, such as:
> 
> - What format the client accepts
> - What format the server is sending
> - Authentication credentials
> - User agent information
> - Caching directives
> 
> **Format:** `key: value`
> 
> **Content Negotiation:** Content negotiation is the process where the client and server agree on what format data should be exchanged in.
> 
> **Process:**
> 
> 1. **Client Request**: Client adds an `Accept` header specifying desired format
>     
>     ```
>     Accept: application/json
>     ```
>     
> 2. **Server Response**: Server responds with a `Content-Type` header confirming the format it's sending
>     
>     ```
>     Content-Type: application/json
>     ```
>     
> 
> **Example in Python:**
> 
> ```python
> # Client specifies it wants JSON
> response = requests.get('https://api.example.com',
>                        headers={'accept': 'application/json'})
> 
> # Check what format server sent
> print(response.headers.get('content-type'))  # 'application/json'
> ```

**Q51.** Describe the five categories of HTTP status codes and provide an example use case for each category.

> [!success]- Answer
> 
> |Category|Range|Meaning|Example & Use Case|
> |---|---|---|---|
> |**1XX**|100-199|Informational responses|**100 Continue**: Server received request headers, client should send body|
> |**2XX**|200-299|Successful responses|**200 OK**: Request successful, data returned. **201 Created**: New resource created successfully|
> |**3XX**|300-399|Redirection messages|**301 Moved Permanently**: Resource moved to new URL, client should use new location|
> |**4XX**|400-499|Client error responses|**404 Not Found**: Resource doesn't exist. **401 Unauthorized**: Authentication required|
> |**5XX**|500-599|Server error responses|**500 Internal Server Error**: Server encountered an error processing request|
> 
> **Practical interpretation:**
> 
> - **2XX** = Success, proceed as normal
> - **4XX** = You (client) did something wrong, fix your request
> - **5XX** = Server has a problem, not your fault
> - **3XX** = Resource moved, follow redirect
> - **1XX** = Informational, keep waiting

**Q52.** What are the three main types of Web APIs? Compare their key characteristics and ideal use cases.

> [!success]- Answer
> 
> |API Type|Focus|Characteristics|Ideal Use Cases|
> |---|---|---|---|
> |**SOAP**|Strict & formal design|- Rigid standards<br>- XML-based<br>- Built-in security<br>- Stateful operations|- Enterprise applications<br>- Banking systems<br>- Payment gateways<br>- Systems requiring high security|
> |**REST**|Simplicity & scalability|- Stateless<br>- Uses standard HTTP<br>- Flexible formats (JSON, XML)<br>- Easy to learn|- Most web services<br>- Mobile apps<br>- Public APIs<br>- Microservices|
> |**GraphQL**|Flexibility & performance|- Query exactly what you need<br>- Single endpoint<br>- Reduces over-fetching<br>- Strongly typed|- Complex data requirements<br>- Mobile apps (reduce bandwidth)<br>- Apps with varied data needs<br>- Real-time applications|
> 
> **Summary:**
> 
> - **SOAP**: Old-school, very formal, for serious enterprise stuff
> - **REST**: Most popular, simple, works great for most use cases
> - **GraphQL**: Newer, very flexible, optimized for performance

**Q53.** Explain the two methods for adding query parameters to a request in Python. Which is preferred and why?

> [!success]- Answer **Method 1: Append to URL String**
> 
> ```python
> response = requests.get('http://api.example.com/users?id=123&active=true')
> ```
> 
> **Pros:** Simple for very basic cases **Cons:**
> 
> - Manual URL encoding required for special characters
> - Error-prone for complex parameters
> - Hard to maintain and read
> 
> **Method 2: Use params Argument (PREFERRED)**
> 
> ```python
> query_params = {'id': 123, 'active': True}
> response = requests.get('http://api.example.com/users', params=query_params)
> ```
> 
> **Pros:**
> 
> - Automatic URL encoding
> - Cleaner, more readable code
> - Easier to maintain and modify
> - Less error-prone
> - Better for dynamic parameters
> 
> **Why Method 2 is Preferred:**
> 
> 1. **Automatic encoding**: Handles special characters, spaces, etc.
> 2. **Cleaner syntax**: Separates URL from parameters
> 3. **More Pythonic**: Uses dictionaries which are easy to manipulate
> 4. **Maintainable**: Easy to add/remove parameters programmatically
> 
> **Example showing the difference:**
> 
> ```python
> # If you need to add "name=John Doe" (space in name)
> 
> # Method 1 - You need to handle encoding yourself
> # requests.get('http://api.com/users?name=John Doe')  # WRONG!
> 
> # Method 2 - Automatic encoding
> params = {'name': 'John Doe'}
> requests.get('http://api.com/users', params=params)  # CORRECT!
> # Results in: http://api.com/users?name=John+Doe
> ```

**Q54.** How do you access and check status codes using the requests library? Provide examples using both numeric values and the requests.codes constants.

> [!success]- Answer **Method 1: Using Numeric Values**
> 
> ```python
> response = requests.get('https://api.example.com/users/123')
> 
> # Direct comparison
> if response.status_code == 200:
>     print("Success!")
> 
> # Check if successful (any 2XX)
> if 200 <= response.status_code < 300:
>     print("Successful response")
> 
> # Check specific errors
> if response.status_code == 404:
>     print("Resource not found")
> elif response.status_code == 500:
>     print("Server error")
> ```
> 
> **Method 2: Using requests.codes (More Readable)**
> 
> ```python
> response = requests.get('https://api.example.com/wrong/path')
> 
> # Using named constants
> if response.status_code == requests.codes.ok:  # 200
>     print("Success!")
> 
> if response.status_code == requests.codes.not_found:  # 404
>     print("Resource not found")
> 
> if response.status_code == requests.codes.internal_server_error:  # 500
>     print("Server error")
> ```
> 
> **Accessing the Status Code:**
> 
> ```python
> response = requests.get('https://api.example.com')
> 
> # Get the numeric code
> print(response.status_code)  # 200
> 
> # Check if request was successful (raises exception if 4XX or 5XX)
> response.raise_for_status()  # Raises HTTPError if status >= 400
> ```
> 
> **Best Practice Example:**
> 
> ```python
> response = requests.get('https://api.example.com/data')
> 
> if response.status_code == requests.codes.ok:
>     data = response.json()
>     print("Data received successfully")
> elif response.status_code == requests.codes.not_found:
>     print("Endpoint doesn't exist")
> else:
>     print(f"Error: {response.status_code}")
> ```

**Q55.** Describe the structure of HTTP request and response messages. What are the main components of each?

> [!success]- Answer **HTTP Request Message Structure:**
> 
> ```
> [Start-line]
> GET /users/123 HTTP/1.1
> 
> [Headers]
> Host: api.example.com
> Accept: application/json
> Authorization: Bearer token123
> 
> [Empty line]
> 
> [Body - Optional]
> {"name": "John", "email": "john@example.com"}
> ```
> 
> **Request Components:**
> 
> 1. **Start-line**: HTTP method + URL path + HTTP version
> 2. **Headers**: Metadata (key-value pairs) about the request
> 3. **Empty line**: Separates headers from body
> 4. **Body**: Data being sent (optional, mainly for POST/PUT)
> 
> **HTTP Response Message Structure:**
> 
> ```
> [Start-line]
> HTTP/1.1 200 OK
> 
> [Headers]
> Content-Type: application/json
> Content-Length: 1234
> Date: Mon, 27 Oct 2025 12:00:00 GMT
> 
> [Empty line]
> 
> [Body]
> {"id": 123, "name": "John", "email": "john@example.com"}
> ```
> 
> **Response Components:**
> 
> 1. **Start-line**: HTTP version + Status code + Status text
> 2. **Headers**: Metadata about the response
> 3. **Empty line**: Separates headers from body
> 4. **Body**: The actual data/content returned
> 
> **Key Difference:**
> 
> - **Request start-line**: Method + Path (what you want to do)
> - **Response start-line**: Status code (what happened)
> 
> **Python Example:**
> 
> ```python
> response = requests.get('https://api.example.com/users/123',
>                        headers={'Accept': 'application/json'})
> 
> # Access response components
> print(response.status_code)  # From start-line: 200
> print(response.headers)       # Response headers
> print(response.text)          # Response body
> ```

---

## Section D: Code Analysis & Scenarios (25 points)

_2.5 points each | 10 questions total_

**Q56.** What does the following code do? Explain the output.

```python
import requests

query_params = {'category': 'electronics', 'price_max': 500}
response = requests.get('https://api.store.com/products', params=query_params)
print(response.url)
```

> [!success]- Answer **What it does:** This code makes a GET request to an API endpoint for products, filtering results using query parameters.
> 
> **Step-by-step:**
> 
> 1. Creates a dictionary with two query parameters: `category='electronics'` and `price_max=500`
> 2. Makes a GET request to `https://api.store.com/products`
> 3. The `params` argument automatically adds query parameters to the URL
> 4. Prints the final URL that was actually requested
> 
> **Output:**
> 
> ```
> https://api.store.com/products?category=electronics&price_max=500
> ```
> 
> **Key Points:**
> 
> - The `params` argument handles URL encoding automatically
> - Query parameters are added after the `?` symbol
> - Multiple parameters are separated by `&`
> - The `response.url` attribute shows the complete URL with parameters

**Q57.** Identify and fix all issues in the following code:

```python
import requests

response = requests.get('https://api.example.com/data')
if response.status_code = 200:
    print(response.data)
else:
    print("Error:", response.status_code)
```

> [!success]- Answer **Issues Found:**
> 
> 1. **Line 4**: Using assignment operator `=` instead of comparison operator `==`
> 2. **Line 5**: Using `response.data` which doesn't exist - should be `response.text` or `response.json()`
> 
> **Corrected Code:**
> 
> ```python
> import requests
> 
> response = requests.get('https://api.example.com/data')
> if response.status_code == 200:  # Fixed: == instead of =
>     print(response.text)  # Fixed: .text instead of .data
> else:
>     print("Error:", response.status_code)
> ```
> 
> **Alternative (better) corrections:**
> 
> ```python
> import requests
> 
> response = requests.get('https://api.example.com/data')
> 
> # Option 1: Using requests.codes for readability
> if response.status_code == requests.codes.ok:
>     print(response.json())  # If expecting JSON data
> else:
>     print("Error:", response.status_code)
> 
> # Option 2: Using try-except with raise_for_status()
> try:
>     response.raise_for_status()  # Raises exception if error
>     data = response.json()
>     print(data)
> except requests.exceptions.HTTPError as e:
>     print("Error:", e)
> ```

**Q58.** Complete the following code to send a POST request creating a new user with name "Alice" and email "alice@example.com", and include an Accept header requesting JSON format:

```python
import requests

url = 'https://api.example.com/users'
# YOUR CODE HERE
```

> [!success]- Answer **Completed Code:**
> 
> ```python
> import requests
> 
> url = 'https://api.example.com/users'
> 
> # Create user data
> user_data = {
>     'name': 'Alice',
>     'email': 'alice@example.com'
> }
> 
> # Create headers
> headers = {
>     'Accept': 'application/json'
> }
> 
> # Send POST request
> response = requests.post(url, data=user_data, headers=headers)
> 
> # Check if successful
> if response.status_code == 201:  # 201 = Created
>     print("User created successfully!")
>     print(response.json())
> else:
>     print(f"Error: {response.status_code}")
> ```
> 
> **Key Elements:**
> 
> 1. Use `requests.post()` for creating resources
> 2. Pass data using the `data` argument
> 3. Pass headers using the `headers` argument
> 4. Both arguments accept dictionaries
> 5. Check for status code 201 (Created) for successful POST requests
> 
> **Alternative (more concise):**
> 
> ```python
> response = requests.post(
>     'https://api.example.com/users',
>     data={'name': 'Alice', 'email': 'alice@example.com'},
>     headers={'Accept': 'application/json'}
> )
> ```

**Q59.** You're building an application that needs to retrieve product information from an API. The API endpoint is `https://api.shop.com/products` and requires:

- Product ID as a query parameter named `id`
- Category filter as a query parameter named `category`
- An API key in the headers as `X-API-Key`

Write code to retrieve a product with ID 12345 in category "electronics" using API key "abc123xyz".

> [!success]- Answer **Solution:**
> 
> ```python
> import requests
> 
> # Define the endpoint
> url = 'https://api.shop.com/products'
> 
> # Define query parameters
> params = {
>     'id': 12345,
>     'category': 'electronics'
> }
> 
> # Define headers with API key
> headers = {
>     'X-API-Key': 'abc123xyz'
> }
> 
> # Make the GET request
> response = requests.get(url, params=params, headers=headers)
> 
> # Check response and handle data
> if response.status_code == requests.codes.ok:
>     product_data = response.json()
>     print("Product retrieved successfully!")
>     print(product_data)
> elif response.status_code == 404:
>     print("Product not found")
> elif response.status_code == 401:
>     print("Invalid API key")
> else:
>     print(f"Error: {response.status_code}")
> 
> # Print the final URL to verify
> print(f"Request URL: {response.url}")
> ```
> 
> **Expected URL:**
> 
> ```
> https://api.shop.com/products?id=12345&category=electronics
> ```
> 
> **Key Points:**
> 
> - Use `params` for query parameters (automatically encoded)
> - Use `headers` for authentication and metadata
> - Always check status code before processing data
> - Use `response.json()` to parse JSON data

**Q60.** Analyze this code and explain what would happen if the API returns a 404 status code:

```python
import requests

response = requests.get('https://api.example.com/users/999')
data = response.json()
print(f"User name: {data['name']}")
```

> [!success]- Answer **What Happens:** If the API returns a 404 status code (Not Found), this code will likely **crash with an error**.
> 
> **Problems:**
> 
> 1. **No status code check**: Code assumes request always succeeds
> 2. **Line 4**: `response.json()` might fail if response body isn't valid JSON
> 3. **Line 5**: Accessing `data['name']` will fail if JSON parsing failed or if the structure is different
> 
> **Potential Errors:**
> 
> - `JSONDecodeError`: If 404 response doesn't contain valid JSON
> - `KeyError`: If JSON doesn't have a 'name' key
> 
> **Fixed Code with Proper Error Handling:**
> 
> ```python
> import requests
> 
> response = requests.get('https://api.example.com/users/999')
> 
> # Check status code first
> if response.status_code == requests.codes.ok:
>     try:
>         data = response.json()
>         print(f"User name: {data['name']}")
>     except (ValueError, KeyError) as e:
>         print(f"Error parsing response: {e}")
> elif response.status_code == 404:
>     print("User not found")
> else:
>     print(f"Error: {response.status_code}")
> ```
> 
> **Better Approach Using raise_for_status():**
> 
> ```python
> import requests
> 
> try:
>     response = requests.get('https://api.example.com/users/999')
>     response.raise_for_status()  # Raises exception for 4XX/5XX
>     
>     data = response.json()
>     print(f"User name: {data.get('name', 'Unknown')}")  # Safer access
>     
> except requests.exceptions.HTTPError as e:
>     if response.status_code == 404:
>         print("User not found")
>     else:
>         print(f"HTTP error: {e}")
> except requests.exceptions.RequestException as e:
>     print(f"Request failed: {e}")
> ```

**Q61.** What's wrong with this approach to adding query parameters? How would you improve it?

```python
import requests

user_id = 42
status = "active"
sort = "name"

url = f"https://api.example.com/users?id={user_id}&status={status}&sort={sort}"
response = requests.get(url)
```

> [!success]- Answer **Problems with This Approach:**
> 
> 1. **No URL encoding**: Special characters in parameter values won't be properly encoded
> 2. **Hard to maintain**: Adding/removing parameters requires string manipulation
> 3. **Error-prone**: Easy to make syntax errors with `&` and `=`
> 4. **Not Pythonic**: Doesn't use the built-in capabilities of requests library
> 5. **Security risk**: Values aren't sanitized (could lead to injection if values come from user input)
> 
> **Example of Failure:**
> 
> ```python
> search_term = "John & Jane"  # Contains special character &
> url = f"https://api.example.com/search?q={search_term}"
> # Results in: ?q=John & Jane
> # Server interprets this as TWO parameters: q=John and Jane=(empty)
> ```
> 
> **Improved Code:**
> 
> ```python
> import requests
> 
> user_id = 42
> status = "active"
> sort = "name"
> 
> # Use params argument with dictionary
> params = {
>     'id': user_id,
>     'status': status,
>     'sort': sort
> }
> 
> response = requests.get('https://api.example.com/users', params=params)
> ```
> 
> **Benefits of Improved Version:**
> 
> - ✅ Automatic URL encoding
> - ✅ Easy to add/remove parameters (just modify dictionary)
> - ✅ More readable and maintainable
> - ✅ Handles special characters correctly
> - ✅ Parameters can be easily built dynamically
> 
> **Dynamic Parameters Example:**
> 
> ```python
> params = {'id': user_id}
> 
> # Conditionally add parameters
> if status:
>     params['status'] = status
> if sort:
>     params['sort'] = sort
> 
> response = requests.get('https://api.example.com/users', params=params)
> ```

**Q62.** Debug this code that attempts to update a user's profile via PUT request:

```python
import requests

url = 'https://api.example.com/users/123'
update_data = {'name': 'John Doe', 'age': 30}

response = requests.get(url, data=update_data)

if response.status_code == 200:
    print("Profile updated!")
```

> [!success]- Answer **Bugs Found:**
> 
> 1. **Line 6**: Using `requests.get()` instead of `requests.put()`
>     - GET is for retrieving data, not updating
>     - Should use PUT for updates
> 2. **Line 8**: Checking for status code 200
>     - PUT requests typically return 200 (OK) or 204 (No Content)
>     - Should handle both possibilities
> 
> **Corrected Code:**
> 
> ```python
> import requests
> 
> url = 'https://api.example.com/users/123'
> update_data = {'name': 'John Doe', 'age': 30}
> 
> # Fixed: Use PUT method for updates
> response = requests.put(url, data=update_data)
> 
> # Fixed: Check for appropriate success codes
> if response.status_code in [200, 204]:
>     print("Profile updated successfully!")
> elif response.status_code == 404:
>     print("User not found")
> else:
>     print(f"Update failed: {response.status_code}")
> ```
> 
> **Even Better Version (with headers and error handling):**
> 
> ```python
> import requests
> 
> url = 'https://api.example.com/users/123'
> update_data = {'name': 'John Doe', 'age': 30}
> headers = {'Content-Type': 'application/json'}
> 
> try:
>     # Use json parameter if API expects JSON
>     response = requests.put(url, json=update_data, headers=headers)
>     response.raise_for_status()
>     
>     if response.status_code == 200:
>         updated_user = response.json()
>         print("Profile updated!")
>         print(updated_user)
>     elif response.status_code == 204:
>         print("Profile updated (no content returned)")
>         
> except requests.exceptions.HTTPError as e:
>     print(f"HTTP error occurred: {e}")
> except requests.exceptions.RequestException as e:
>     print(f"Request failed: {e}")
> ```
> 
> **Key Differences Between data and json Parameters:**
> 
> ```python
> # Using data (sends as form-encoded)
> requests.put(url, data={'key': 'value'})
> 
> # Using json (sends as JSON, automatically sets Content-Type header)
> requests.put(url, json={'key': 'value'})
> ```

**Q63.** Write code to interact with a weather API that requires:

- Endpoint: `https://api.weather.com/current`
- Query parameters: `city` and `units` (metric or imperial)
- Header: `Accept: application/json`
- Check if response is successful and handle common errors (404, 500)
- If successful, extract and print temperature from JSON response (assume structure: `{"temp": 25.5}`)

> [!success]- Answer **Complete Solution:**
> 
> ```python
> import requests
> 
> # Define API endpoint and parameters
> url = 'https://api.weather.com/current'
> params = {
>     'city': 'London',
>     'units': 'metric'
> }
> headers = {
>     'Accept': 'application/json'
> }
> 
> # Make GET request
> response = requests.get(url, params=params, headers=headers)
> 
> # Handle different status codes
> if response.status_code == requests.codes.ok:
>     try:
>         # Parse JSON response
>         weather_data = response.json()
>         temperature = weather_data.get('temp')
>         
>         if temperature is not None:
>             print(f"Current temperature in London: {temperature}°C")
>         else:
>             print("Temperature data not available")
>             
>     except ValueError:
>         print("Error: Response is not valid JSON")
>         
> elif response.status_code == 404:
>     print("Error: City not found or endpoint doesn't exist")
>     
> elif response.status_code == 500:
>     print("Error: Server error occurred. Please try again later")
>     
> else:
>     print(f"Unexpected error: {response.status_code}")
> 
> # Print request URL for debugging
> print(f"Request URL: {response.url}")
> ```
> 
> **Production-Ready Version with Better Error Handling:**
> 
> ```python
> import requests
> from requests.exceptions import RequestException, HTTPError, JSONDecodeError
> 
> def get_weather(city, units='metric'):
>     """
>     Fetch current weather for a city.
>     
>     Args:
>         city (str): City name
>         units (str): 'metric' or 'imperial'
>         
>     Returns:
>         float: Temperature or None if error
>     """
>     url = 'https://api.weather.com/current'
>     params = {'city': city, 'units': units}
>     headers = {'Accept': 'application/json'}
>     
>     try:
>         response = requests.get(url, params=params, headers=headers, timeout=10)
>         response.raise_for_status()  # Raises HTTPError for bad status
>         
>         weather_data = response.json()
>         return weather_data.get('temp')
>         
>     except HTTPError as e:
>         if response.status_code == 404:
>             print(f"City '{city}' not found")
>         elif response.status_code == 500:
>             print("Server error. Please try again later")
>         else:
>             print(f"HTTP error: {e}")
>         return None
>         
>     except JSONDecodeError:
>         print("Error: Invalid JSON response")
>         return None
>         
>     except RequestException as e:
>         print(f"Request failed: {e}")
>         return None
> 
> # Usage
> temp = get_weather('London', 'metric')
> if temp is not None:
>     print(f"Temperature: {temp}°C")
> ```
> 
> **Key Features:**
> 
> - ✅ Proper error handling for all scenarios
> - ✅ Uses .get() to safely access dictionary keys
> - ✅ Timeout specified to prevent hanging
> - ✅ Organized as reusable function
> - ✅ Detailed error messages for debugging

**Q64.** Scenario: You're testing an API and receive this response. Analyze what went wrong:

```python
response = requests.post('https://api.example.com/data')
print(response.status_code)  # Output: 400
print(response.headers.get('content-type'))  # Output: application/json
print(response.json())  # Output: {"error": "Missing required field: name"}
```

What's the problem and how would you fix it?

> [!success]- Answer **Analysis:**
> 
> **Status Code 400** = Bad Request (Client Error)
> 
> - The client (your code) sent a malformed or incomplete request
> - The 4XX category indicates YOU did something wrong, not the server
> 
> **Error Message:** "Missing required field: name"
> 
> - The API requires a field called "name" in the request
> - You didn't include any data in the POST request
> 
> **Problem:** The POST request is being sent without any data. The API expects data to be sent (specifically a "name" field), but nothing was included.
> 
> **Fixed Code:**
> 
> ```python
> # Include required data
> data = {
>     'name': 'Example Name'
> }
> 
> response = requests.post('https://api.example.com/data', data=data)
> 
> if response.status_code == 201:  # Created
>     print("Data created successfully!")
>     print(response.json())
> elif response.status_code == 400:
>     print("Bad request:")
>     print(response.json())  # Show error details
> else:
>     print(f"Error: {response.status_code}")
> ```
> 
> **Better Solution with All Recommended Practices:**
> 
> ```python
> import requests
> 
> # Prepare data
> data = {
>     'name': 'Example Name',
>     # Add any other required fields
> }
> 
> # Add appropriate headers
> headers = {
>     'Content-Type': 'application/json',
>     'Accept': 'application/json'
> }
> 
> # Make request (use json parameter for JSON data)
> response = requests.post(
>     'https://api.example.com/data', 
>     json=data,  # Automatically serializes and sets Content-Type
>     headers=headers
> )
> 
> # Handle response
> try:
>     response.raise_for_status()
>     result = response.json()
>     print("Success:", result)
> except requests.exceptions.HTTPError:
>     if response.status_code == 400:
>         error_details = response.json()
>         print(f"Bad Request: {error_details.get('error')}")
>     else:
>         print(f"HTTP Error: {response.status_code}")
> ```
> 
> **Key Lessons:**
> 
> - 400 errors mean YOU need to fix your request
> - Always send required data with POST/PUT
> - Read error messages - they tell you what's missing
> - Use `json=data` instead of `data=data` when sending JSON

**Q65.** Compare these two code snippets. Which is better and why?

**Snippet A:**

```python
response = requests.get('https://api.example.com/users')
if response.status_code == 200:
    users = response.json()
```

**Snippet B:**

```python
response = requests.get('https://api.example.com/users')
try:
    response.raise_for_status()
    users = response.json()
except requests.exceptions.HTTPError as e:
    print(f"Error: {e}")
```

> [!success]- Answer **Snippet B is better.** Here's why:
> 
> **Problems with Snippet A:**
> 
> 1. **Incomplete error handling**: Only checks for 200, ignores all other status codes
> 2. **Silent failures**: If status code is not 200, `users` is never defined
> 3. **No JSON parsing protection**: Could fail if response isn't valid JSON
> 4. **Doesn't handle other success codes**: 201, 204, etc. are ignored
> 
> **What happens in Snippet A if status is 404?**
> 
> ```python
> # If status_code is 404:
> # - The if block doesn't execute
> # - users variable is never created
> # - Next line using `users` will crash with NameError
> ```
> 
> **Advantages of Snippet B:**
> 
> 1. **Comprehensive error handling**: `raise_for_status()` handles ALL error codes (4XX, 5XX)
> 2. **Explicit exception handling**: Clear what to do when errors occur
> 3. **Cleaner code**: Less status code checking
> 4. **Best practice**: Using try-except for error handling
> 
> **How raise_for_status() works:**
> 
> ```python
> # Raises HTTPError if status_code >= 400
> # Does nothing if status_code < 400 (success)
> response.raise_for_status()
> ```
> 
> **Even Better Version (Most Robust):**
> 
> ```python
> import requests
> from requests.exceptions import HTTPError, RequestException
> 
> try:
>     response = requests.get('https://api.example.com/users', timeout=10)
>     response.raise_for_status()
>     users = response.json()
>     
>     # Process users
>     for user in users:
>         print(user)
>         
> except HTTPError as e:
>     # Handle HTTP errors (4XX, 5XX)
>     print(f"HTTP error occurred: {e}")
>     if response.status_code == 404:
>         print("Endpoint not found")
>     elif response.status_code == 401:
>         print("Authentication required")
>         
> except requests.exceptions.JSONDecodeError:
>     # Handle invalid JSON
>     print("Response is not valid JSON")
>     
> except RequestException as e:
>     # Handle connection errors, timeouts, etc.
>     print(f"Request failed: {e}")
> ```
> 
> **Comparison Summary:**
> 
> |Aspect|Snippet A|Snippet B|
> |---|---|---|
> |Error handling|Limited (only 200)|Comprehensive (all 4XX/5XX)|
> |Code clarity|More verbose|Cleaner, more Pythonic|
> |Robustness|Weak|Strong|
> |Best practice|No|Yes|
> |Production-ready|No|More suitable|

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)

1. B | 2. C | 3. C | 4. C | 5. B | 6. C | 7. B | 8. B | 9. C | 10. A
2. C | 12. B | 13. C | 14. B | 15. C | 16. C | 17. C | 18. B | 19. B | 20. D
3. B | 22. B | 23. D | 24. B | 25. C | 26. B | 27. C | 28. B | 29. B | 30. C

### Section B: True/False (15 points)

31. F | 32. T | 33. F | 34. F | 35. T | 36. F | 37. F | 38. T | 39. T | 40. F
32. F | 42. T | 43. F | 44. F | 45. T

### Section C: Short Answer (30 points)

- Questions 46-55 require detailed written answers
- Award partial credit for incomplete but correct information
- Full points require comprehensive explanations with examples

### Section D: Code Analysis (25 points)

- Questions 56-65 require code analysis, debugging, or completion
- Partial credit for identifying some issues or providing partial solutions
- Full points require complete, working solutions with explanations

---

## Quick Study Guide

### Essential Code Patterns

**Basic GET Request:**

```python
import requests
response = requests.get('https://api.example.com/endpoint')
```

**GET with Query Parameters:**

```python
params = {'key1': 'value1', 'key2': 'value2'}
response = requests.get('https://api.example.com/endpoint', params=params)
```

**POST with Data:**

```python
data = {'name': 'John', 'email': 'john@example.com'}
response = requests.post('https://api.example.com/users', data=data)
```

**POST with JSON:**

```python
data = {'name': 'John', 'email': 'john@example.com'}
response = requests.post('https://api.example.com/users', json=data)
```

**With Headers:**

```python
headers = {'Authorization': 'Bearer token123', 'Accept': 'application/json'}
response = requests.get('https://api.example.com/data', headers=headers)
```

**PUT Request:**

```python
data = {'name': 'John Doe'}
response = requests.put('https://api.example.com/users/123', data=data)
```

**DELETE Request:**

```python
response = requests.delete('https://api.example.com/users/123')
```

**Checking Status Codes:**

```python
# Method 1: Numeric
if response.status_code == 200:
    print("Success!")

# Method 2: Named constants
if response.status_code == requests.codes.ok:
    print("Success!")

# Method 3: raise_for_status
try:
    response.raise_for_status()
except requests.exceptions.HTTPError as e:
    print(f"Error: {e}")
```

**Accessing Response Data:**

```python
response.status_code      # Status code (int)
response.text             # Response body as string
response.json()           # Parse JSON response
response.headers          # Response headers (dict-like)
response.url              # Final URL (after redirects)
```

**Reading Headers:**

```python
# Dictionary-style (raises KeyError if missing)
content_type = response.headers['content-type']

# Using .get() (returns None if missing)
content_type = response.headers.get('content-type')
```

---

## Key Concepts Summary

### URL Components

|Component|Example|Description|
|---|---|---|
|Protocol|`https://`|HTTP or HTTPS|
|Domain|`api.example.com`|Server address|
|Port|`:443`|Connection port (often omitted)|
|Path|`/users/123`|Specific resource|
|Query|`?id=5&sort=name`|Additional parameters|

### HTTP Verbs

|Verb|Purpose|Has Body?|Idempotent?|
|---|---|---|---|
|GET|Read/Retrieve|No|Yes|
|POST|Create|Yes|No|
|PUT|Update/Replace|Yes|Yes|
|DELETE|Remove|No|Yes|

### Status Code Categories

|Range|Category|Meaning|Example|
|---|---|---|---|
|1XX|Informational|Processing continues|100 Continue|
|2XX|Success|Request succeeded|200 OK, 201 Created|
|3XX|Redirection|Further action needed|301 Moved|
|4XX|Client Error|Client did something wrong|404 Not Found|
|5XX|Server Error|Server encountered error|500 Internal Error|

### Common Headers

|Header|Direction|Purpose|
|---|---|---|
|Accept|Request|What format client wants|
|Content-Type|Response|What format server is sending|
|Authorization|Request|Authentication credentials|
|User-Agent|Request|Client application info|

### requests Library Methods

|Method|Purpose|Key Arguments|
|---|---|---|
|`requests.get()`|Retrieve data|`params`, `headers`|
|`requests.post()`|Create resource|`data` or `json`, `headers`|
|`requests.put()`|Update resource|`data` or `json`, `headers`|
|`requests.delete()`|Remove resource|`headers`|

---

## Common Mistakes to Avoid

❌ **Using assignment (=) instead of comparison (==) in if statements**

```python
if response.status_code = 200:  # WRONG!
if response.status_code == 200:  # CORRECT
```

❌ **Using response.data instead of response.text or response.json()**

```python
print(response.data)    # WRONG - no such attribute
print(response.text)    # CORRECT - string
print(response.json())  # CORRECT - parsed JSON
```

❌ **Manually building URLs with query parameters**

```python
url = f"http://api.com?id={id}&name={name}"  # WRONG - no encoding
requests.get('http://api.com', params={'id': id, 'name': name})  # CORRECT
```

❌ **Not checking status codes before processing response**

```python
data = response.json()  # WRONG - might fail if error status
if response.status_code == 200:
    data = response.json()  # CORRECT
```

❌ **Using GET for operations that modify data**

```python
requests.get(url, data=data)  # WRONG - GET shouldn't have body
requests.post(url, data=data)  # CORRECT - use POST for creation
```

❌ **Forgetting the data/json argument in POST/PUT**

```python
requests.post(url)  # WRONG - no data sent
requests.post(url, data={'key': 'value'})  # CORRECT
```

❌ **Using bracket notation for headers that might not exist**

```python
type = response.headers['content-type']  # WRONG - raises KeyError if missing
type = response.headers.get('content-type')  # CORRECT - returns None if missing
```

❌ **Confusing Accept and Content-Type headers**

```python
# Accept = what YOU want to receive
# Content-Type = what THEY are sending
```

---

## Study Strategy (7-Day Plan)

### Day 1-2: Foundations

- [ ] What is an API, client-server model
- [ ] Types of Web APIs (REST, SOAP, GraphQL)
- [ ] urllib vs requests libraries
- [ ] Basic requests.get() syntax
- **Practice**: Make 10 simple GET requests

### Day 3: URLs and Parameters

- [ ] URL anatomy (protocol, domain, path, query)
- [ ] Query parameters (both methods)
- [ ] When to use params vs manual URL building
- **Practice**: Build URLs with various query parameters

### Day 4: HTTP Verbs

- [ ] Four main verbs: GET, POST, PUT, DELETE
- [ ] When to use each verb
- [ ] Using data argument
- [ ] Difference between data and json parameters
- **Practice**: Write code for all four verb types

### Day 5: Status Codes

- [ ] Five status code categories
- [ ] Common codes (200, 404, 500)
- [ ] Accessing status codes in requests
- [ ] Using requests.codes constants
- [ ] raise_for_status() method
- **Practice**: Write error handling for different status codes

### Day 6: Headers

- [ ] What headers are and their purpose
- [ ] Content negotiation (Accept & Content-Type)
- [ ] Adding headers to requests
- [ ] Reading response headers
- [ ] Dictionary access vs .get() method
- **Practice**: Add headers to requests and read responses

### Day 7: Integration & Review

- [ ] Request/response message anatomy
- [ ] Complete workflow examples
- [ ] Error handling best practices
- [ ] Code debugging exercises
- **Practice**: Complete coding scenarios, take practice quiz

---

## Final Exam Checklist

### Before the Exam:

- [ ] Review all URL components and can identify each
- [ ] Memorize the four main HTTP verbs and their purposes
- [ ] Know all five status code categories
- [ ] Understand difference between Accept and Content-Type
- [ ] Can write basic GET, POST, PUT, DELETE requests from memory
- [ ] Know how to add query parameters (params argument)
- [ ] Know how to add headers to requests
- [ ] Can access status codes and response data
- [ ] Understand error handling with try-except
- [ ] Know difference between response.text and response.json()

### During the Exam:

- [ ] Read each question carefully
- [ ] For code questions, trace through line by line
- [ ] Check for common mistakes (= vs ==, .data vs .text)
- [ ] Verify HTTP verb matches the operation
- [ ] Ensure query parameters use params argument
- [ ] Check status code handling is appropriate
- [ ] Look for missing error handling
- [ ] Verify headers are added correctly

### Quick Mental Checks:

- **Creating?** → Use POST
- **Reading?** → Use GET
- **Updating?** → Use PUT
- **Deleting?** → Use DELETE
- **Adding parameters?** → Use `params` argument
- **Sending data?** → Use `data` or `json` argument
- **Adding headers?** → Use `headers` argument
- **Success?** → Status code 2XX
- **Client error?** → Status code 4XX
- **Server error?** → Status code 5XX

---

## Additional Resources

- **Official requests documentation**: https://requests.readthedocs.io/
- **HTTP status codes**: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
- **REST API best practices**: Research RESTful API design patterns
- **Practice APIs**: Use public APIs like JSONPlaceholder for practice

---

**Good luck with your exam! 🚀**

Remember: APIs are about communication between applications. Focus on understanding the request/response cycle, proper use of HTTP verbs, and always check status codes before processing data!

---

#datacamp #apis #python #rest #http #certification #exam-prep