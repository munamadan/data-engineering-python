
## Section A: Multiple Choice Questions (90 points)

_2 points each | 45 questions total_

### Chapter 1: API Fundamentals & Request Basics (Questions 1-20)

**Q1.** What does API stand for?

- [ ] A) Application Process Interface
- [x] B) Application Programming Interface
- [ ] C) Automated Programming Interface
- [ ] D) Application Protocol Interface

> [!success]- Answer **Correct Answer: B) Application Programming Interface**
> 
> API stands for Application Programming Interface. It is a set of communication rules and abilities that enables interactions between software applications.

**Q2.** In Web APIs, what protocol is used for communication over the internet?

- [ ] A) FTP
- [ ] B) SMTP
- [x] C) HTTP
- [ ] D) SSH

> [!success]- Answer **Correct Answer: C) HTTP**
> 
> Web APIs communicate over the internet using HTTP (HyperText Transfer Protocol), which enables the request/response cycle between client and server.

**Q3.** Which type of Web API focuses on simplicity and scalability and is the most common?

- [ ] A) SOAP
- [ ] B) GraphQL
- [x] C) REST
- [ ] D) XML-RPC

> [!success]- Answer **Correct Answer: C) REST**
> 
> REST (Representational State Transfer) focuses on simplicity and scalability and is the most common API architecture.

**Q4.** Which Python library for working with APIs is bundled with Python but is not very developer-friendly?

- [ ] A) requests
- [x] B) urllib
- [ ] C) httplib
- [ ] D) api-client

> [!success]- Answer **Correct Answer: B) urllib**
> 
> urllib is bundled with Python and is powerful but not very developer-friendly. The requests library is easier to use and recommended.

**Q5.** What is the recommended Python library for making API requests?

- [ ] A) urllib
- [ ] B) http.client
- [x] C) requests
- [ ] D) api-wrapper

> [!success]- Answer **Correct Answer: C) requests**
> 
> The requests library has many powerful built-in features and is easier to use, making it the recommended choice for API work in Python.

**Q6.** What does URL stand for?

- [ ] A) Universal Resource Loader
- [x] B) Uniform Resource Locator
- [ ] C) Unified Response Language
- [ ] D) User Request Locator

> [!success]- Answer **Correct Answer: B) Uniform Resource Locator**
> 
> URL stands for Uniform Resource Locator - the structured address to an API resource.

**Q7.** In the URL `http://350.5th-ave.com/unit/243`, what is `http://` called?

- [ ] A) Domain
- [x] B) Protocol
- [ ] C) Path
- [ ] D) Query parameter

> [!success]- Answer **Correct Answer: B) Protocol**
> 
> The protocol (http:// or https://) is the means of transportation that defines how data is transmitted.

**Q8.** What is the recommended way to add query parameters using the requests library?

- [ ] A) Append them directly to the URL string
- [x] B) Use the `params` argument with a dictionary
- [ ] C) Add them in the request body
- [ ] D) Use the `query` argument

> [!success]- Answer **Correct Answer: B) Use the `params` argument with a dictionary**
> 
> Using the `params` argument with a dictionary is recommended because it's cleaner and automatically handles URL encoding.

**Q9.** Which HTTP verb is used to retrieve/read a resource?

- [ ] A) POST
- [ ] B) PUT
- [x] C) GET
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: C) GET**
> 
> GET is used to retrieve or read a resource from the server, like checking the contents of a mailbox.

**Q10.** Which HTTP verb is used to create a new resource?

- [ ] A) GET
- [x] B) POST
- [ ] C) PUT
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: B) POST**
> 
> POST is used to create a new resource on the server, like dropping a new package in the mailbox.

**Q11.** Which HTTP verb replaces or updates an existing resource?

- [ ] A) GET
- [ ] B) POST
- [x] C) PUT
- [ ] D) PATCH

> [!success]- Answer **Correct Answer: C) PUT**
> 
> PUT is used to update or replace an existing resource, like replacing all packages in a mailbox with a new one.

**Q12.** Which HTTP verb removes a resource?

- [ ] A) GET
- [ ] B) POST
- [ ] C) PUT
- [x] D) DELETE

> [!success]- Answer **Correct Answer: D) DELETE**
> 
> DELETE is used to remove a resource from the server, like removing all packages from the mailbox.

**Q13.** How many total HTTP verbs exist?

- [ ] A) 4
- [ ] B) 6
- [x] C) 9
- [ ] D) 12

> [!success]- Answer **Correct Answer: C) 9**
> 
> There are 9 HTTP verbs in total, but for simple REST APIs only 4 are relevant (GET, POST, PUT, DELETE).

**Q14.** What argument is used to send data with a POST request in the requests library?

- [ ] A) params
- [ ] B) body
- [x] C) data
- [ ] D) payload

> [!success]- Answer **Correct Answer: C) data**
> 
> Use the `data` argument to pass data to a POST or PUT request: `requests.post(url, data={"key": "value"})`.

**Q15.** What HTTP status code indicates a successful request?

- [ ] A) 100
- [x] B) 200
- [ ] C) 404
- [ ] D) 500

> [!success]- Answer **Correct Answer: B) 200**
> 
> Status code 200 means "OK" - the request was successful. It's part of the 2XX category for successful responses.

**Q16.** What does status code 404 mean?

- [ ] A) Internal Server Error
- [x] B) Not Found
- [ ] C) Unauthorized
- [ ] D) Bad Request

> [!success]- Answer **Correct Answer: B) Not Found**
> 
> 404 means "Not Found" - the requested resource doesn't exist on the server.

**Q17.** Status codes in the 5XX range indicate:

- [ ] A) Successful responses
- [ ] B) Redirection messages
- [ ] C) Client error responses
- [x] D) Server error responses

> [!success]- Answer **Correct Answer: D) Server error responses**
> 
> 5XX status codes indicate server errors - something went wrong on the server side while processing the request.

**Q18.** What HTTP header is used by the client to specify the desired response format?

- [ ] A) Content-Type
- [ ] B) Accept
- [ ] C) Authorization
- [ ] D) User-Agent

> [!success]- Answer **Correct Answer: B) Accept**
> 
> The `Accept` header tells the server what format the client wants to receive (like `application/json`).

**Q19.** What does the HTTP header `Content-Type` specify?

- [ ] A) The format the client wants to receive
- [x] B) The format the server is sending
- [ ] C) The authentication credentials
- [ ] D) The API version

> [!success]- Answer **Correct Answer: B) The format the server is sending**
> 
> The `Content-Type` header in the response tells the client what format the server is sending.

**Q20.** How do you access the status code of a response in the requests library?

- [ ] A) response.code
- [ ] B) response.status
- [x] C) response.status_code
- [ ] D) response.http_code

> [!success]- Answer **Correct Answer: C) response.status_code**
> 
> Use `response.status_code` to access the numeric status code: `if response.status_code == 200:`

### Chapter 2: Authentication, JSON & Error Handling (Questions 21-45)

**Q21.** What does API authentication help protect?

- [ ] A) The server hardware
- [x] B) Sensitive data
- [ ] C) Network bandwidth
- [ ] D) Database structure

> [!success]- Answer **Correct Answer: B) Sensitive data**
> 
> APIs often protect sensitive data that shouldn't be publicly accessible. Authentication ensures only authorized users can access protected resources.

**Q22.** Which authentication method has the easiest implementation (⭐⭐⭐⭐)?

- [ ] A) OAuth 2.0
- [ ] B) JWT Authentication
- [x] C) Basic Authentication
- [ ] D) API Key/Token

> [!success]- Answer **Correct Answer: C) Basic Authentication**
> 
> Basic Authentication has the highest ease of implementation rating (⭐⭐⭐⭐), making it the easiest to implement.

**Q23.** Which authentication method has the highest security rating (⭐⭐⭐⭐⭐)?

- [ ] A) Basic Authentication
- [ ] B) API Key/Token
- [ ] C) JWT Authentication
- [x] D) OAuth 2.0

> [!success]- Answer **Correct Answer: D) OAuth 2.0**
> 
> OAuth 2.0 has the highest security rating (⭐⭐⭐⭐⭐) according to the authentication methods comparison table.

**Q24.** What does the `auth` parameter in requests automatically add?

- [ ] A) An API key header
- [x] B) A Basic Authentication header
- [ ] C) A Bearer token
- [ ] D) OAuth credentials

> [!success]- Answer **Correct Answer: B) A Basic Authentication header**
> 
> Using `requests.get(url, auth=('username', 'password'))` automatically adds a Basic Authentication header before sending the request.

**Q25.** What is the format for passing an API token using the Bearer authorization header?

- [ ] A) `'Authorization': 'Token faaa1c97...'`
- [x] B) `'Authorization': 'Bearer faaa1c97...'`
- [ ] C) `'Bearer': 'faaa1c97...'`
- [ ] D) `'Token': 'Bearer faaa1c97...'`

> [!success]- Answer **Correct Answer: B) `'Authorization': 'Bearer faaa1c97...'`**
> 
> The correct format is: `headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}`

**Q26.** What does JSON stand for?

- [ ] A) Java Standard Object Notation
- [x] B) JavaScript Object Notation
- [ ] C) Joint Syntax Object Network
- [ ] D) Java Serialized Object Notation

> [!success]- Answer **Correct Answer: B) JavaScript Object Notation**
> 
> JSON stands for JavaScript Object Notation, though it's language-independent and widely supported.

**Q27.** Which of the following is a characteristic of JSON mentioned in the chapter?

- [ ] A) Binary format
- [x] B) Widely supported
- [ ] C) Requires compilation
- [ ] D) Language-specific

> [!success]- Answer **Correct Answer: B) Widely supported**
> 
> The chapter lists JSON characteristics as: widely supported, human readable, and machine usable.

**Q28.** What does `json.dumps()` do?

- [ ] A) Decodes a JSON string to a Python object
- [x] B) Encodes a Python object to a JSON string
- [ ] C) Dumps JSON data to a file
- [ ] D) Validates JSON format

> [!success]- Answer **Correct Answer: B) Encodes a Python object to a JSON string**
> 
> `json.dumps(album)` encodes a Python object to a JSON string for data exchange.

**Q29.** What does `json.loads()` do?

- [ ] A) Loads JSON from a file
- [ ] B) Encodes a Python object to JSON
- [x] C) Decodes a JSON string to a Python object
- [ ] D) Saves JSON to disk

> [!success]- Answer **Correct Answer: C) Decodes a JSON string to a Python object**
> 
> `json.loads(string)` decodes a JSON string back to a Python object.

**Q30.** What method decodes JSON responses in the requests library?

- [ ] A) `response.decode()`
- [x] B) `response.json()`
- [ ] C) `response.parse()`
- [ ] D) `response.to_dict()`

> [!success]- Answer **Correct Answer: B) `response.json()`**
> 
> Use `data = response.json()` to automatically decode JSON response into a Python object.

**Q31.** When sending JSON data, which parameter should be used with requests.post()?

- [ ] A) `data=`
- [ ] B) `body=`
- [x] C) `json=`
- [ ] D) `payload=`

> [!success]- Answer **Correct Answer: C) `json=`**
> 
> Use the `json=` parameter: `requests.post(url, json=playlist)` to automatically serialize and send JSON data.

**Q32.** What content-type header is automatically set when using the `json=` parameter?

- [ ] A) `text/plain`
- [x] B) `application/json`
- [ ] C) `application/xml`
- [ ] D) `text/html`

> [!success]- Answer **Correct Answer: B) `application/json`**
> 
> When using `json=playlist`, the `Content-Type` header is automatically set to `application/json`.

**Q33.** Which of the following formats are mentioned as alternatives to JSON?

- [ ] A) PDF, DOC, TXT
- [x] B) XML, CSV, YAML
- [ ] C) HTML, CSS, JS
- [ ] D) PNG, JPEG, GIF

> [!success]- Answer **Correct Answer: B) XML, CSV, YAML**
> 
> The chapter lists "Other formats: XML, CSV, YAML" as alternatives to JSON.

**Q34.** What do 4xx status codes indicate?

- [ ] A) Server errors
- [x] B) Client errors
- [ ] C) Successful responses
- [ ] D) Redirections

> [!success]- Answer **Correct Answer: B) Client errors**
> 
> 4xx status codes indicate client errors - issues on the client's end that need to be fixed in the request.

**Q35.** What do 5xx status codes indicate?

- [ ] A) Client errors
- [x] B) Server errors
- [ ] C) Successful responses
- [ ] D) Authentication errors

> [!success]- Answer **Correct Answer: B) Server errors**
> 
> 5xx status codes indicate server errors - problems on the server that should be fixed by the API administrator.

**Q36.** What does status code 401 mean?

- [ ] A) Not Found
- [ ] B) Internal Server Error
- [x] C) Unauthorized - lacks valid authentication credentials
- [ ] D) Too Many Requests

> [!success]- Answer **Correct Answer: C) Unauthorized - lacks valid authentication credentials**
> 
> 401 Unauthorized means the request lacks valid authentication credentials for the requested resource.

**Q37.** What does status code 404 indicate?

- [ ] A) Server is overloaded
- [ ] B) Authentication failed
- [x] C) The server cannot find the requested resource
- [ ] D) Too many requests

> [!success]- Answer **Correct Answer: C) The server cannot find the requested resource**
> 
> 404 Not Found indicates that the server cannot find the resource that was requested.

**Q38.** What does status code 429 mean?

- [ ] A) Server error
- [ ] B) Authentication required
- [x] C) The client has sent too many requests
- [ ] D) Bad gateway

> [!success]- Answer **Correct Answer: C) The client has sent too many requests**
> 
> 429 Too Many Requests means the client has sent too many requests in a given amount of time (rate limit exceeded).

**Q39.** What does status code 500 indicate?

- [ ] A) Client error
- [x] B) The server experienced an unexpected issue
- [ ] C) Authentication failed
- [ ] D) Resource not found

> [!success]- Answer **Correct Answer: B) The server experienced an unexpected issue**
> 
> 500 Internal Server Error means the server experienced an unexpected issue which prevents it from responding.

**Q40.** In the error handling example, what condition is checked to detect an error?

- [ ] A) `if r.status_code == 404:`
- [x] B) `if r.status_code >= 400:`
- [ ] C) `if r.status_code == 500:`
- [ ] D) `if r.error:`

> [!success]- Answer **Correct Answer: B) `if r.status_code >= 400:`**
> 
> The chapter shows checking `if r.status_code >= 400:` to detect any error (both 4xx and 5xx).

**Q41.** Which exception should be imported to handle connection errors?

- [ ] A) HTTPError
- [x] B) ConnectionError
- [ ] C) TimeoutError
- [ ] D) ValueError

> [!success]- Answer **Correct Answer: B) ConnectionError**
> 
> Import and catch `ConnectionError` to handle connection problems: `from requests.exceptions import ConnectionError`.

**Q42.** What does the `raise_for_status()` method do?

- [ ] A) Prints the status code
- [ ] B) Returns the status code
- [x] C) Enables raising exceptions for returned error status codes
- [ ] D) Clears the status code

> [!success]- Answer **Correct Answer: C) Enables raising exceptions for returned error status codes**
> 
> `r.raise_for_status()` enables raising exceptions for returned error status codes (>= 400).

**Q43.** Which exception is caught to handle error responses from the API server?

- [ ] A) ConnectionError
- [ ] B) ValueError
- [x] C) HTTPError
- [ ] D) RuntimeError

> [!success]- Answer **Correct Answer: C) HTTPError**
> 
> Catch `HTTPError` to handle error responses from the API server: `except HTTPError as http_err:`

**Q44.** What does status code 502 (Bad Gateway) indicate?

- [ ] A) Client error
- [x] B) The API server could not reach another server
- [ ] C) Authentication failed
- [ ] D) Resource not found

> [!success]- Answer **Correct Answer: B) The API server could not reach another server**
> 
> 502 Bad Gateway means the API server could not successfully reach another server it needed to complete the response.

**Q45.** What does status code 504 (Gateway Timeout) indicate?

- [ ] A) Client timeout
- [x] B) The gateway did not get a response from the upstream server in time
- [ ] C) Authentication timeout
- [ ] D) Resource locked

> [!success]- Answer **Correct Answer: B) The gateway did not get a response from the upstream server in time**
> 
> 504 Gateway Timeout means the server (acting as gateway) did not get a response from the upstream server in time.

---

## Section B: True/False Questions (30 points)

_1 point each | 30 questions total_

### Chapter 1 Coverage (Questions 46-60)

**Q46.** The requests library is bundled with Python and doesn't need to be installed separately. **T/F**

> [!success]- Answer **False** - The requests library is NOT bundled with Python. urllib is the library that comes bundled with Python.

**Q47.** REST is the most common API architecture. **T/F**

> [!success]- Answer **True** - REST focuses on simplicity and scalability and is the most common API architecture.

**Q48.** The GET HTTP verb is used to create new resources. **T/F**

> [!success]- Answer **False** - GET is used to read/retrieve resources. POST is used to create new resources.

**Q49.** Status code 500 indicates a successful response. **T/F**

> [!success]- Answer **False** - Status code 500 means "Internal Server Error" (server error). Status code 200 indicates success.

**Q50.** Query parameters are separated from the path in a URL by a question mark (?). **T/F**

> [!success]- Answer **True** - The question mark (?) separates the path from query parameters in a URL.

**Q51.** The Accept header is used by the server to tell the client what format it's sending. **T/F**

> [!success]- Answer **False** - The Accept header is used by the CLIENT to tell the server what format it wants. The server uses Content-Type to indicate what it's sending.

**Q52.** There are exactly 4 HTTP verbs in total. **T/F**

> [!success]- Answer **False** - There are 9 HTTP verbs in total, but only 4 (GET, POST, PUT, DELETE) are commonly used for simple REST APIs.

**Q53.** The params argument in requests.get() is used to add query parameters. **T/F**

> [!success]- Answer **True** - The `params` argument accepts a dictionary and automatically adds query parameters to the URL.

**Q54.** Status codes in the 2XX range indicate successful responses. **T/F**

> [!success]- Answer **True** - The 2XX category (200-299) represents successful responses.

**Q55.** The DELETE HTTP verb is used to update existing resources. **T/F**

> [!success]- Answer **False** - DELETE is used to remove resources. PUT is used to update/replace existing resources.

**Q56.** urllib is easier to use than requests for making API calls. **T/F**

> [!success]- Answer **False** - urllib is powerful but not very developer-friendly. The requests library is easier to use.

**Q57.** GraphQL focuses on flexibility and performance optimization. **T/F**

> [!success]- Answer **True** - GraphQL focuses on flexibility and is optimized for performance.

**Q58.** The response.headers.get() method will raise an error if the header key doesn't exist. **T/F**

> [!success]- Answer **False** - The `.get()` method returns None if the key doesn't exist, making it safer than dictionary-style access.

**Q59.** In Web APIs, the server sends a request message to the client. **T/F**

> [!success]- Answer **False** - The CLIENT sends the request message to the server. The server returns a response message.

**Q60.** The data argument is used in requests.post() to send data in the request body. **T/F**

> [!success]- Answer **True** - The `data` argument is used to pass data to POST and PUT requests.

### Chapter 2 Coverage (Questions 61-75)

**Q61.** Basic Authentication automatically adds a header when using the `auth` parameter. **T/F**

> [!success]- Answer **True** - Using `auth=('username', 'password')` automatically adds a Basic Authentication header.

**Q62.** OAuth 2.0 has the highest security rating among the authentication methods shown. **T/F**

> [!success]- Answer **True** - OAuth 2.0 has the highest security rating (⭐⭐⭐⭐⭐) in the comparison table.

**Q63.** JSON stands for JavaScript Object Notation. **T/F**

> [!success]- Answer **True** - JSON stands for JavaScript Object Notation.

**Q64.** `json.dumps()` decodes a JSON string to a Python object. **T/F**

> [!success]- Answer **False** - `json.dumps()` ENCODES a Python object to a JSON string. `json.loads()` decodes.

**Q65.** XML is mentioned as an alternative format to JSON. **T/F**

> [!success]- Answer **True** - The chapter lists XML, CSV, and YAML as alternative formats to JSON.

**Q66.** The `json=` argument in requests.post() automatically sets the content-type header to application/json. **T/F**

> [!success]- Answer **True** - Using `json=playlist` automatically sets `Content-Type: application/json`.

**Q67.** 4xx status codes indicate server errors. **T/F**

> [!success]- Answer **False** - 4xx codes indicate CLIENT errors. 5xx codes indicate server errors.

**Q68.** A 401 status code means "Not Found". **T/F**

> [!success]- Answer **False** - 401 means "Unauthorized". 404 means "Not Found".

**Q69.** A 500 status code should be fixed by the API administrator. **T/F**

> [!success]- Answer **True** - 5xx server errors should be fixed by the API administrator, not the client.

**Q70.** ConnectionError is used to catch HTTP error responses from the API server. **T/F**

> [!success]- Answer **False** - ConnectionError catches connection problems. HTTPError catches error responses from the server.

**Q71.** The chapter shows two methods for passing an API key: query parameter and Bearer header. **T/F**

> [!success]- Answer **True** - The chapter demonstrates both query parameter (`params`) and Bearer header (`Authorization`) methods.

**Q72.** response.json() decodes the JSON response into a Python object. **T/F**

> [!success]- Answer **True** - `response.json()` automatically decodes the JSON response into a Python object.

**Q73.** A 429 status code means the server is experiencing an internal error. **T/F**

> [!success]- Answer **False** - 429 means "Too Many Requests" (rate limit). 500 is for internal server errors.

**Q74.** The chapter recommends checking `if r.status_code >= 400:` to detect errors. **T/F**

> [!success]- Answer **True** - The error handling example shows: `if r.status_code >= 400:` to detect errors.

**Q75.** raise_for_status() must be called before making the request. **T/F**

> [!success]- Answer **False** - raise_for_status() is called AFTER getting the response: `r = requests.get(...); r.raise_for_status()`.

---

## Section C: Short Answer Questions (50 points)

_5 points each | 10 questions total_

**Q76.** Compare the three types of Web APIs mentioned in the chapter (SOAP, REST, GraphQL). What does each focus on?

> [!success]- Answer **SOAP:**
> 
> - Focus: Strict and formal API design
> - Use case: Enterprise applications
> 
> **REST:**
> 
> - Focus: Simplicity & scalability
> - Use case: Most common API architecture
> 
> **GraphQL:**
> 
> - Focus: Flexibility
> - Feature: Optimized for performance

**Q77.** Explain the five components of a URL using the example from the chapter: `http://350.5th-ave.com/unit/243?floor=77&elevator=True`

> [!success]- Answer
> 
> 1. **Protocol** (`http://`) - The means of transportation
> 2. **Domain** (`350.5th-ave.com`) - The street address of the office building
> 3. **Port** (not shown, but would be after domain with `:`) - The gate or door to use
> 4. **Path** (`/unit/243`) - The specific office unit inside the building
> 5. **Query** (`?floor=77&elevator=True`) - Any additional instructions
> 
> Note: In this example, the port is omitted (uses default).

**Q78.** List the four main HTTP verbs covered in the chapter. For each, state its action and describe what it does using the mailbox analogy.

> [!success]- Answer
> 
> |Verb|Action|Mailbox Analogy|
> |---|---|---|
> |**GET**|Read|Check the mailbox contents|
> |**POST**|Create|Drop a new package in the mailbox|
> |**PUT**|Update|Replace all packages with a new one|
> |**DELETE**|Delete|Remove all packages from the mailbox|
> 
> Note: There are 9 HTTP verbs in total, but these 4 are relevant for simple REST APIs.

**Q79.** List the four authentication methods from the chapter comparison table with their ease of implementation ratings and security ratings.

> [!success]- Answer
> 
> |Method|Ease|Security|
> |---|---|---|
> |**Basic Authentication**|⭐⭐⭐⭐|⭐|
> |**API key/token Authentication**|⭐⭐⭐|⭐⭐⭐|
> |**JWT Authentication**|⭐⭐|⭐⭐⭐⭐|
> |**OAuth 2.0**|⭐|⭐⭐⭐⭐⭐|
> 
> **Key tip from chapter:** Check the documentation of the API you are using to learn which method to use!

**Q80.** Explain the two methods shown in the chapter for passing an API key/token. Provide the code for each.

> [!success]- Answer **Method 1: Using a query parameter**
> 
> ```python
> params = {'access_token': 'faaa1c97bd3f4bd9b024c708c979feca'}
> requests.get('http://api.music-catalog.com/albums', params=params)
> ```
> 
> Creates URL: `http://api.music-catalog.com/albums?access_token=faaa1c97...`
> 
> **Method 2: Using the "Bearer" authorization header**
> 
> ```python
> headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}
> requests.get('http://api.music-catalog.com/albums', headers=headers)
> ```
> 
> Token passed in Authorization header (more secure).

**Q81.** What are the three characteristics of JSON mentioned in the chapter? Also list the three alternative formats mentioned.

> [!success]- Answer **JSON Characteristics:**
> 
> 1. **Widely supported** - Works across platforms
> 2. **Human readable** - Easy to understand
> 3. **Machine usable** - Easy to parse
> 
> **Alternative Formats:**
> 
> - XML
> - CSV
> - YAML

**Q82.** Describe what `json.dumps()` and `json.loads()` do using the album example from the chapter.

> [!success]- Answer **json.dumps() - Encode Python to JSON:**
> 
> ```python
> import json
> album = {'id': 42, 'title': "Back in Black"}
> string = json.dumps(album)  # Encodes a python object to a JSON string
> ```
> 
> Result: `'{"id": 42, "title": "Back in Black"}'` (string)
> 
> **json.loads() - Decode JSON to Python:**
> 
> ```python
> album = json.loads(string)  # Decodes a JSON string to a Python object
> ```
> 
> Result: `{'id': 42, 'title': "Back in Black"}` (Python dict)