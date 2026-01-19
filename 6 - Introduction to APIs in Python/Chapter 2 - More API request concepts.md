## API Authentication

### Why Authentication?

APIs often protect **sensitive data** that shouldn't be publicly accessible. Authentication ensures that only authorized users can access protected resources.

### Authentication Methods Comparison

|Method|Ease of Implementation|Security Rating|Use Case|
|---|---|---|---|
|**Basic Authentication**|⭐⭐⭐⭐ (Easiest)|⭐ (Least secure)|Simple internal APIs|
|**API Key/Token**|⭐⭐⭐|⭐⭐⭐|Most common for public APIs|
|**JWT Authentication**|⭐⭐|⭐⭐⭐⭐|Modern web applications|
|**OAuth 2.0**|⭐ (Most complex)|⭐⭐⭐⭐⭐ (Most secure)|Third-party app access|

> [!tip] Important Always check the API documentation to learn which authentication method to use!

---

## Basic Authentication

### How It Works

Basic Authentication sends a username and password encoded in Base64 with each request.

### Implementation with requests

```python
import requests

# Method 1: Using the auth parameter (RECOMMENDED)
response = requests.get(
    'http://api.music-catalog.com',
    auth=('username', 'password')
)

# This automatically adds a Basic Authentication header:
# Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

> [!warning] Security Consideration Basic authentication sends credentials with every request. Always use HTTPS (not HTTP) to encrypt the connection!

---

## API Key/Token Authentication

API keys or tokens are strings that identify and authenticate the application/user.

### Method 1: Query Parameter (Less Secure)

```python
# URL example
# http://api.music-catalog.com/albums?access_token=faaa1c97bd3f4bd9b024c708c979feca

# Python implementation
params = {'access_token': 'faaa1c97bd3f4bd9b024c708c979feca'}
response = requests.get('http://api.music-catalog.com/albums', params=params)
```

> [!warning] Less Secure Query parameters appear in URLs, which may be logged in browser history or server logs.

### Method 2: Bearer Authorization Header (MORE SECURE - RECOMMENDED)

```python
# Using Authorization header with Bearer token
headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}
response = requests.get('http://api.music-catalog.com/albums', headers=headers)
```

> [!tip] Best Practice Use the Bearer header method - it's more secure and follows industry standards.

### Key Differences

|Method|Security|Visibility|Best For|
|---|---|---|---|
|**Query Parameter**|Lower|Visible in URL|Quick testing only|
|**Bearer Header**|Higher|Hidden in headers|Production use|

---

## Working with Structured Data

### JSON - JavaScript Object Notation

**JSON** is the most common format for API data exchange.

#### Why JSON?

- ✅ **Widely supported** across all programming languages
- ✅ **Human readable** - easy to understand
- ✅ **Machine usable** - easy to parse
- ✅ **Lightweight** - efficient data transfer

#### Other Data Formats

While JSON is most common, APIs may also use:

- **XML**: Older, more verbose format
- **CSV**: For tabular data
- **YAML**: Configuration files

---

## Python JSON Operations

### The json Module

Python's built-in `json` module handles JSON encoding and decoding.

```python
import json

# Python dictionary
album = {'id': 42, 'title': 'Back in Black'}

# Encode Python object to JSON string
json_string = json.dumps(album)
print(json_string)  # '{"id": 42, "title": "Back in Black"}'

# Decode JSON string to Python object
album_dict = json.loads(json_string)
print(album_dict)   # {'id': 42, 'title': 'Back in Black'}
```

### Key Functions

|Function|Purpose|Input|Output|
|---|---|---|---|
|`json.dumps()`|Encode to JSON|Python object|JSON string|
|`json.loads()`|Decode from JSON|JSON string|Python object|

> [!info] Memory Tip
> 
> - `dumps` = **dump** to **s**tring
> - `loads` = **load** from **s**tring

---

## Requesting JSON Data

### Content Negotiation with Accept Header

```python
import requests

# Without headers - might get plain text
response = requests.get('http://api.music-catalog.com/lyrics')
print(response.text)
# Output: N' I never miss Cause I'm a problem child - AC/DC, Problem Child

# WITH accept header - request JSON format
response = requests.get(
    'http://api.music-catalog.com/lyrics',
    headers={'accept': 'application/json'}
)

# Print raw JSON text
print(response.text)
# Output: '{"artist": "AC/DC", "lyric": "N' I never miss...", "track": "Problem Child"}'

# Parse JSON into Python dictionary
data = response.json()  # Automatically decodes JSON
print(data['artist'])   # AC/DC
print(data['track'])    # Problem Child
```

### response.json() Method

The `response.json()` method is a convenience function that:

1. Checks if response is JSON
2. Automatically decodes it to Python objects (dict/list)
3. Raises an error if JSON is invalid

```python
response = requests.get('http://api.example.com/data')

# These are equivalent:
data = json.loads(response.text)  # Manual decoding
data = response.json()             # Automatic decoding (RECOMMENDED)
```

---

## Sending JSON Data

### Using the json Parameter

```python
import requests

# Create Python dictionary
playlist = {
    "name": "Road trip",
    "genre": "rock",
    "private": "true"
}

# Send as JSON using the json parameter
response = requests.post(
    "http://api.music-catalog.com/playlists",
    json=playlist  # Automatically converts to JSON and sets Content-Type
)

# Verify the Content-Type header was set
request = response.request
print(request.headers['content-type'])  # application/json
```

### What the json Parameter Does

When you use `json=data`:

1. Automatically calls `json.dumps()` on your data
2. Sets `Content-Type: application/json` header
3. Sends properly formatted JSON in request body

### json vs data Parameter

```python
# Using json parameter (RECOMMENDED for JSON)
requests.post(url, json={'key': 'value'})
# ✅ Automatically serializes to JSON
# ✅ Sets Content-Type: application/json
# ✅ Proper encoding

# Using data parameter (for form data)
requests.post(url, data={'key': 'value'})
# ❌ Sends as form-encoded data
# ❌ Content-Type: application/x-www-form-urlencoded
```

> [!important] Remember
> 
> - Use `json=` parameter when sending JSON data to APIs
> - Use `data=` parameter when sending form data

---

## Error Handling

### Types of Errors

#### 1. Connection Errors

Problems establishing a connection to the server:

- Network is down
- Server is unreachable
- DNS resolution failed
- Timeout occurred

#### 2. HTTP Errors

Server returned an error status code:

- **4XX Client Errors**: You did something wrong
- **5XX Server Errors**: Server has a problem

---

## HTTP Error Status Codes

### 4XX - Client Errors

**These indicate issues on the client's end.**

|Code|Name|Meaning|Resolution|
|---|---|---|---|
|**400**|Bad Request|Malformed request syntax|Fix the request format|
|**401**|Unauthorized|Missing/invalid authentication|Provide valid credentials|
|**403**|Forbidden|Authenticated but not authorized|Check permissions|
|**404**|Not Found|Resource doesn't exist|Verify the URL/endpoint|
|**429**|Too Many Requests|Rate limit exceeded|Slow down requests, wait|

> [!info] Client Errors = Your Fault Fix the request - check authentication, URL, parameters, data format

### 5XX - Server Errors

**These arise from problems on the server side.**

|Code|Name|Meaning|Resolution|
|---|---|---|---|
|**500**|Internal Server Error|Unexpected server issue|Contact API administrator|
|**502**|Bad Gateway|Server couldn't reach upstream server|Wait and retry|
|**503**|Service Unavailable|Server temporarily overloaded|Retry with backoff|
|**504**|Gateway Timeout|Upstream server didn't respond in time|Retry later|

> [!info] Server Errors = Not Your Fault Should be fixed by API administrator - you can only retry or wait

---

## Handling Errors in Code

### Method 1: Check Status Codes Manually

```python
import requests

url = 'http://api.music-catalog.com/albums'
response = requests.get(url)

if response.status_code >= 400:
    # Oops, something went wrong
    print(f"Error: {response.status_code}")
    
    # Handle specific errors
    if response.status_code == 401:
        print("Authentication required!")
    elif response.status_code == 404:
        print("Resource not found!")
    elif response.status_code >= 500:
        print("Server error - try again later")
else:
    # All fine, process the response
    data = response.json()
    print(data)
```

### Method 2: Handle Connection Errors with try-except

```python
import requests
from requests.exceptions import ConnectionError

url = 'http://api.music-catalog.com/albums'

try:
    response = requests.get(url)
    print(response.status_code)
except ConnectionError as conn_err:
    print(f'Connection Error! {conn_err}.')
```

---

## Using raise_for_status() - BEST PRACTICE

The `raise_for_status()` method automatically raises an exception for error status codes (4XX and 5XX).

### Complete Error Handling Pattern

```python
import requests
from requests.exceptions import ConnectionError, HTTPError

try:
    # Make the request
    response = requests.get("http://api.music-catalog.com/albums")
    
    # Raise exception if status code >= 400
    response.raise_for_status()
    
    # If we get here, request was successful
    print(response.status_code)
    data = response.json()
    print(data)
    
except ConnectionError as conn_err:
    # Handle connection problems
    print(f'Connection Error! {conn_err}.')
    
except HTTPError as http_err:
    # Handle HTTP error responses (4XX, 5XX)
    print(f'HTTP error occurred: {http_err}')
```

### How raise_for_status() Works

```python
response = requests.get(url)
response.raise_for_status()

# If status_code < 400: Does nothing (continues normally)
# If status_code >= 400: Raises HTTPError exception
```

### Benefits of raise_for_status()

✅ Cleaner code - less status code checking  
✅ Automatic error detection  
✅ Exception-based error handling (Pythonic)  
✅ Catches all error codes (4XX and 5XX)

---

## Complete Error Handling Example

```python
import requests
from requests.exceptions import ConnectionError, HTTPError, Timeout, RequestException

def fetch_albums(api_url, token):
    """
    Fetch albums from API with comprehensive error handling.
    """
    headers = {'Authorization': f'Bearer {token}'}
    
    try:
        # Make request with timeout
        response = requests.get(api_url, headers=headers, timeout=10)
        
        # Raise exception for bad status codes
        response.raise_for_status()
        
        # Parse JSON response
        albums = response.json()
        return albums
        
    except ConnectionError:
        print("Failed to connect to the server. Check your internet connection.")
        return None
        
    except Timeout:
        print("Request timed out. Server took too long to respond.")
        return None
        
    except HTTPError as http_err:
        # Handle specific HTTP errors
        if response.status_code == 401:
            print("Authentication failed. Check your API token.")
        elif response.status_code == 404:
            print("Endpoint not found. Check the API URL.")
        elif response.status_code == 429:
            print("Rate limit exceeded. Please wait before retrying.")
        elif response.status_code >= 500:
            print(f"Server error: {http_err}")
        else:
            print(f"HTTP error: {http_err}")
        return None
        
    except RequestException as req_err:
        # Catch any other requests-related errors
        print(f"An error occurred: {req_err}")
        return None

# Usage
albums = fetch_albums('http://api.music-catalog.com/albums', 'your-token-here')
if albums:
    print(f"Successfully retrieved {len(albums)} albums")
```

---

## Common Exception Types

|Exception|When It's Raised|Example Cause|
|---|---|---|
|`ConnectionError`|Can't connect to server|Network down, wrong URL|
|`HTTPError`|Status code >= 400|404, 500, 401, etc.|
|`Timeout`|Request takes too long|Slow server, network issues|
|`RequestException`|Base exception for all|Catches any request error|

> [!tip] Exception Hierarchy `RequestException` is the parent class of all requests exceptions. Use it as a catch-all at the end of your try-except blocks.

---

## Quick Reference: Complete API Request Pattern

```python
import requests
from requests.exceptions import ConnectionError, HTTPError, Timeout

# Configuration
API_URL = 'https://api.example.com/data'
API_TOKEN = 'your-api-token-here'

# Setup
headers = {
    'Authorization': f'Bearer {API_TOKEN}',
    'Accept': 'application/json'
}

params = {'category': 'music', 'limit': 10}

# Make request with error handling
try:
    response = requests.get(
        API_URL,
        headers=headers,
        params=params,
        timeout=10
    )
    
    response.raise_for_status()
    data = response.json()
    
    # Process data
    for item in data:
        print(item)
        
except ConnectionError:
    print("Connection failed")
except Timeout:
    print("Request timed out")
except HTTPError as e:
    print(f"HTTP error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

---

## Key Takeaways

### Authentication

✅ Always check API documentation for required method  
✅ Use `auth=` parameter for Basic Authentication  
✅ Use Bearer tokens in headers (not query parameters)  
✅ Always use HTTPS for authentication

### JSON Data

✅ Use `response.json()` to decode JSON responses  
✅ Use `json=data` parameter to send JSON data  
✅ Set `Accept: application/json` header when requesting JSON

### Error Handling

✅ Always use `try-except` for connection errors  
✅ Use `raise_for_status()` to catch HTTP errors  
✅ Handle specific error codes when appropriate  
✅ Add timeout to prevent hanging requests  
✅ 4XX = client error (fix your request)  
✅ 5XX = server error (not your fault)

---

#datacamp #apis #python #authentication #json #error-handling #certification

# Chapter 2: API Authentication & Data Handling - Practice Quiz

**Total Points: 130**  
**Passing Score: 91+ (70%)**  
**Time Limit: 90 minutes**

---

## Section A: Multiple Choice Questions (60 points)

_2 points each | 30 questions total_

### Topic 1: Authentication Methods (Questions 1-10)

**Q1.** What does API authentication help protect?

- [ ] A) The server hardware
- [ ] B) Sensitive data
- [ ] C) Network bandwidth
- [ ] D) Database structure

> [!success]- Answer **Correct Answer: B) Sensitive data**
> 
> APIs often protect sensitive data that shouldn't be publicly accessible. Authentication ensures only authorized users can access protected resources.

**Q2.** According to the chapter, which authentication method has the easiest implementation?

- [ ] A) OAuth 2.0
- [ ] B) JWT Authentication
- [ ] C) Basic Authentication
- [ ] D) API Key/Token Authentication

> [!success]- Answer **Correct Answer: C) Basic Authentication**
> 
> Basic Authentication is rated with the highest ease of implementation (⭐⭐⭐⭐), making it the easiest method to implement.

**Q3.** Which authentication method has the highest security rating?

- [ ] A) Basic Authentication
- [ ] B) API Key/Token
- [ ] C) JWT Authentication
- [ ] D) OAuth 2.0

> [!success]- Answer **Correct Answer: D) OAuth 2.0**
> 
> OAuth 2.0 has the highest security rating (⭐⭐⭐⭐⭐) according to the comparison table in the chapter.

**Q4.** What should you always check to learn which authentication method to use for an API?

- [ ] A) The server logs
- [ ] B) The API documentation
- [ ] C) The database schema
- [ ] D) The network configuration

> [!success]- Answer **Correct Answer: B) The API documentation**
> 
> The chapter explicitly states: "Check the documentation of the API you are using to learn which method to use for authentication!"

**Q5.** What does the `auth` parameter in requests automatically add?

- [ ] A) An API key header
- [ ] B) A Basic Authentication header
- [ ] C) A Bearer token
- [ ] D) OAuth credentials

> [!success]- Answer **Correct Answer: B) A Basic Authentication header**
> 
> Using `requests.get('http://api.music-catalog.com', auth=('username', 'password'))` automatically adds a Basic Authentication header before sending the request.

**Q6.** In the example `params = {'access_token': 'faaa1c97bd3f4bd9b024c708c979feca'}`, where is the API token being passed?

- [ ] A) In a header
- [ ] B) In the request body
- [ ] C) As a query parameter
- [ ] D) In a cookie

> [!success]- Answer **Correct Answer: C) As a query parameter**
> 
> The chapter shows this creates a URL like `http://api.music-catalog.com/albums?access_token=faaa1c97...` where the token is passed as a query parameter.

**Q7.** What is the format for passing an API token using the Bearer authorization header?

- [ ] A) `'Authorization': 'Token faaa1c97...'`
- [ ] B) `'Authorization': 'Bearer faaa1c97...'`
- [ ] C) `'Bearer': 'faaa1c97...'`
- [ ] D) `'Token': 'Bearer faaa1c97...'`

> [!success]- Answer **Correct Answer: B) `'Authorization': 'Bearer faaa1c97...'`**
> 
> The chapter shows: `headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}`

**Q8.** Which authentication method has a ⭐⭐⭐ rating for both ease of implementation and security?

- [ ] A) Basic Authentication
- [ ] B) API Key/Token Authentication
- [ ] C) JWT Authentication
- [ ] D) OAuth 2.0

> [!success]- Answer **Correct Answer: B) API Key/Token Authentication**
> 
> According to the comparison table, API Key/Token Authentication has ⭐⭐⭐ for ease and ⭐⭐⭐ for security, making it a balanced choice.

**Q9.** How many stars (⭐) does Basic Authentication have for security rating?

- [ ] A) ⭐
- [ ] B) ⭐⭐
- [ ] C) ⭐⭐⭐
- [ ] D) ⭐⭐⭐⭐

> [!success]- Answer **Correct Answer: A) ⭐**
> 
> Basic Authentication has the lowest security rating of just one star (⭐), despite being the easiest to implement.

**Q10.** Which two methods are shown in the chapter for passing an API key/token?

- [ ] A) Query parameter and request body
- [ ] B) Query parameter and Bearer header
- [ ] C) Request body and cookie
- [ ] D) Bearer header and cookie

> [!success]- Answer **Correct Answer: B) Query parameter and Bearer header**
> 
> The chapter demonstrates both methods:
> 
> 1. Query parameter: `params = {'access_token': '...'}`
> 2. Bearer header: `headers = {'Authorization': 'Bearer ...'}`

### Topic 2: JSON & Structured Data (Questions 11-20)

**Q11.** What does JSON stand for?

- [ ] A) Java Standard Object Notation
- [ ] B) JavaScript Object Notation
- [ ] C) Joint Syntax Object Network
- [ ] D) Java Serialized Object Notation

> [!success]- Answer **Correct Answer: B) JavaScript Object Notation**
> 
> The chapter clearly states: "JSON - JavaScript Object Notation"

**Q12.** According to the chapter, which of the following is NOT a characteristic of JSON?

- [ ] A) Widely supported
- [ ] B) Human readable
- [ ] C) Machine usable
- [ ] D) Requires special software to read

> [!success]- Answer **Correct Answer: D) Requires special software to read**
> 
> The chapter lists JSON characteristics as: widely supported, human readable, and machine usable. It doesn't require special software.

**Q13.** Which of the following formats are mentioned in the chapter as alternatives to JSON?

- [ ] A) XML, CSV, YAML
- [ ] B) PDF, DOC, TXT
- [ ] C) HTML, CSS, JS
- [ ] D) PNG, JPEG, GIF

> [!success]- Answer **Correct Answer: A) XML, CSV, YAML**
> 
> The chapter explicitly lists "Other formats: XML, CSV, YAML" as alternatives to JSON.

**Q14.** What does `json.dumps()` do?

- [ ] A) Decodes a JSON string to a Python object
- [ ] B) Encodes a Python object to a JSON string
- [ ] C) Reads JSON from a file
- [ ] D) Validates JSON format

> [!success]- Answer **Correct Answer: B) Encodes a Python object to a JSON string**
> 
> The chapter shows: `string = json.dumps(album) # Encodes a python object to a JSON string`

**Q15.** What does `json.loads()` do?

- [ ] A) Encodes a Python object to JSON
- [ ] B) Decodes a JSON string to a Python object
- [ ] C) Loads JSON from a URL
- [ ] D) Saves JSON to a file

> [!success]- Answer **Correct Answer: B) Decodes a JSON string to a Python object**
> 
> The chapter shows: `album = json.loads(string) # Decodes a JSON string to a Python object`

**Q16.** In the chapter example, what header is used to request JSON-formatted data?

- [ ] A) `content-type: application/json`
- [ ] B) `accept: application/json`
- [ ] C) `format: json`
- [ ] D) `response-type: json`

> [!success]- Answer **Correct Answer: B) `accept: application/json`**
> 
> The chapter shows: `response = requests.get('http://api.music-catalog.com/lyrics', headers={'accept': 'application/json'})`

**Q17.** What method can be used on the response object to decode JSON into a Python object?

- [ ] A) `response.decode()`
- [ ] B) `response.json()`
- [ ] C) `response.parse()`
- [ ] D) `response.to_dict()`

> [!success]- Answer **Correct Answer: B) `response.json()`**
> 
> The chapter demonstrates: `data = response.json()` to decode the JSON response into a Python object.

**Q18.** When sending JSON data, which parameter should be used with requests.post()?

- [ ] A) `data=`
- [ ] B) `body=`
- [ ] C) `json=`
- [ ] D) `payload=`

> [!success]- Answer **Correct Answer: C) `json=`**
> 
> The chapter shows: `response = requests.post("http://api.music-catalog.com/playlists", json=playlist)`

**Q19.** When you use the `json=` argument in requests.post(), what content-type header is automatically set?

- [ ] A) `text/plain`
- [ ] B) `application/json`
- [ ] C) `application/xml`
- [ ] D) `text/html`

> [!success]- Answer **Correct Answer: B) `application/json`**
> 
> The chapter demonstrates: after using `json=playlist`, printing `request.headers['content-type']` shows `application/json`.

**Q20.** In the lyric API example, what was the artist name returned in the JSON response?

- [ ] A) Metallica
- [ ] B) Led Zeppelin
- [ ] C) AC/DC
- [ ] D) Pink Floyd

> [!success]- Answer **Correct Answer: C) AC/DC**
> 
> The chapter shows the response: `{'artist': 'AC/DC', 'lyric': "N' I never miss Cause I'm a problem child", 'track': 'Problem Child'}` and then `print(data['artist'])` outputs `AC/DC`.

### Topic 3: Error Handling (Questions 21-30)

**Q21.** What do 4xx status codes indicate?

- [ ] A) Server errors
- [ ] B) Client errors
- [ ] C) Successful responses
- [ ] D) Redirections

> [!success]- Answer **Correct Answer: B) Client errors**
> 
> The chapter states: "4xx Client Errors - Indicate issues on the client's end"

**Q22.** What do 5xx status codes indicate?

- [ ] A) Client errors
- [ ] B) Server errors
- [ ] C) Successful responses
- [ ] D) Authentication errors

> [!success]- Answer **Correct Answer: B) Server errors**
> 
> The chapter states: "5xx Server Errors - Arises from problems on the server"

**Q23.** According to the chapter, what does a 401 status code mean?

- [ ] A) Not Found
- [ ] B) Internal Server Error
- [ ] C) Unauthorized - lacks valid authentication credentials
- [ ] D) Too Many Requests

> [!success]- Answer **Correct Answer: C) Unauthorized - lacks valid authentication credentials**
> 
> The chapter explicitly defines: "401 Unauthorized - The request lacks valid authentication credentials for the requested resource"

**Q24.** What does a 404 status code indicate?

- [ ] A) Server is overloaded
- [ ] B) Authentication failed
- [ ] C) The server cannot find the requested resource
- [ ] D) Too many requests

> [!success]- Answer **Correct Answer: C) The server cannot find the requested resource**
> 
> The chapter states: "404 Not Found - Indicates that the server cannot find the resource that was requested"

**Q25.** What does a 429 status code mean?

- [ ] A) Server error
- [ ] B) Authentication required
- [ ] C) The client has sent too many requests
- [ ] D) Bad gateway

> [!success]- Answer **Correct Answer: C) The client has sent too many requests**
> 
> The chapter defines: "429 Too Many Requests - The client has sent too many requests in a given amount of time"

**Q26.** What does a 500 status code indicate?

- [ ] A) Client error
- [ ] B) The server experienced an unexpected issue
- [ ] C) Authentication failed
- [ ] D) Resource not found

> [!success]- Answer **Correct Answer: B) The server experienced an unexpected issue**
> 
> The chapter states: "500 Internal Server Error - The server experienced an unexpected issue which prevents it from responding"

**Q27.** In the error handling example, what condition is checked to detect an error?

- [ ] A) `if r.status_code == 404:`
- [ ] B) `if r.status_code >= 400:`
- [ ] C) `if r.status_code == 500:`
- [ ] D) `if r.error:`

> [!success]- Answer **Correct Answer: B) `if r.status_code >= 400:`**
> 
> The chapter shows: `if r.status_code >= 400:` as the condition to check if something went wrong.

**Q28.** Which exception should be imported to handle connection errors?

- [ ] A) `HTTPError`
- [ ] B) `ConnectionError`
- [ ] C) `TimeoutError`
- [ ] D) `ValueError`

> [!success]- Answer **Correct Answer: B) `ConnectionError`**
> 
> The chapter shows: `from requests.exceptions import ConnectionError` and then catches it with `except ConnectionError as conn_err:`

**Q29.** What does the `raise_for_status()` method do?

- [ ] A) Prints the status code
- [ ] B) Returns the status code
- [ ] C) Enables raising exceptions for returned error status codes
- [ ] D) Clears the status code

> [!success]- Answer **Correct Answer: C) Enables raising exceptions for returned error status codes**
> 
> The chapter comments show: `r.raise_for_status() # 2: Enable raising exceptions for returned error statuscodes`

**Q30.** Which exception is caught to handle error responses from the API server?

- [ ] A) `ConnectionError`
- [ ] B) `ValueError`
- [ ] C) `HTTPError`
- [ ] D) `RuntimeError`

> [!success]- Answer **Correct Answer: C) `HTTPError`**
> 
> The chapter shows: `except HTTPError as http_err:` with the comment "# 4: Catch error responses from the API server"

---

## Section B: True/False Questions (15 points)

_1 point each | 15 questions total_

**Q31.** Basic Authentication automatically adds a header when using the `auth` parameter. **T/F**

> [!success]- Answer **True** - The chapter states: "This will automatically add a Basic Authentication header before sending the request" when using `requests.get('http://api.music-catalog.com', auth=('username', 'password'))`

**Q32.** JSON stands for JavaScript Object Notation. **T/F**

> [!success]- Answer **True** - The chapter explicitly states "JSON - JavaScript Object Notation"

**Q33.** `json.dumps()` decodes a JSON string to a Python object. **T/F**

> [!success]- Answer **False** - `json.dumps()` encodes a Python object to a JSON string. `json.loads()` decodes a JSON string to a Python object.

**Q34.** XML is mentioned as an alternative format to JSON. **T/F**

> [!success]- Answer **True** - The chapter lists "Other formats: XML, CSV, YAML" as alternatives to JSON.

**Q35.** The `json=` argument in requests.post() automatically sets the content-type header to application/json. **T/F**

> [!success]- Answer **True** - The chapter demonstrates that after using `json=playlist`, the `request.headers['content-type']` shows `application/json`.

**Q36.** 4xx status codes indicate server errors. **T/F**

> [!success]- Answer **False** - 4xx status codes indicate client errors. 5xx status codes indicate server errors.

**Q37.** A 401 status code means "Not Found". **T/F**

> [!success]- Answer **False** - 401 means "Unauthorized" (lacks valid authentication credentials). 404 means "Not Found".

**Q38.** A 500 status code should be fixed by the API administrator, not the client. **T/F**

> [!success]- Answer **True** - The chapter states: "Resolution: Should be fixed by the API administrator" for 5xx Server Errors.

**Q39.** OAuth 2.0 has the highest security rating among the authentication methods shown. **T/F**

> [!success]- Answer **True** - The authentication comparison table shows OAuth 2.0 with the highest security rating (⭐⭐⭐⭐⭐).

**Q40.** ConnectionError is used to catch HTTP error responses from the API server. **T/F**

> [!success]- Answer **False** - ConnectionError catches connection problems. HTTPError is used to catch error responses from the API server.

**Q41.** The chapter shows two methods for passing an API key: query parameter and Bearer header. **T/F**

> [!success]- Answer **True** - The chapter demonstrates both: using query parameter (`params = {'access_token': '...'}`) and using Bearer header (`headers = {'Authorization': 'Bearer ...'}`).

**Q42.** response.json() decodes the JSON response into a Python object. **T/F**

> [!success]- Answer **True** - The chapter shows: `data = response.json()` decodes the JSON into a Python object.

**Q43.** A 429 status code means the server is experiencing an internal error. **T/F**

> [!success]- Answer **False** - 429 means "Too Many Requests" (client has sent too many requests). 500 is for internal server errors.

**Q44.** The chapter recommends checking `if r.status_code >= 400:` to detect errors. **T/F**

> [!success]- Answer **True** - The error handling example shows: `if r.status_code >= 400: # Oops, something went wrong`

**Q45.** raise_for_status() must be called before making the request. **T/F**

> [!success]- Answer **False** - raise_for_status() is called AFTER getting the response: `r = requests.get(...); r.raise_for_status()`

---

## Section C: Short Answer Questions (30 points)

_3 points each | 10 questions total_

**Q46.** List the four authentication methods shown in the chapter and their ease of implementation ratings (using stars ⭐).

> [!success]- Answer According to the authentication methods table:
> 
> 1. **Basic Authentication**: ⭐⭐⭐⭐ (easiest)
> 2. **API key/token Authentication**: ⭐⭐⭐
> 3. **JWT Authentication**: ⭐⭐
> 4. **OAuth 2.0**: ⭐ (most complex)

**Q47.** Explain the two methods shown in the chapter for passing an API token. Provide the code example for each.

> [!success]- Answer **Method 1: Using Query Parameter**
> 
> ```python
> params = {'access_token': 'faaa1c97bd3f4bd9b024c708c979feca'}
> requests.get('http://api.music-catalog.com/albums', params=params)
> ```
> 
> This creates a URL with the token visible in the query string: `http://api.music-catalog.com/albums?access_token=faaa1c97...`
> 
> **Method 2: Using Bearer Authorization Header**
> 
> ```python
> headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}
> requests.get('http://api.music-catalog.com/albums', headers=headers)
> ```
> 
> This passes the token in the Authorization header using the Bearer scheme.

**Q48.** What are the three characteristics of JSON mentioned in the chapter?

> [!success]- Answer According to the chapter, JSON has three key characteristics:
> 
> 1. **Widely supported** - Works across different platforms and languages
> 2. **Human readable** - Easy for people to read and understand
> 3. **Machine usable** - Easy for computers to parse and generate

**Q49.** Describe what `json.dumps()` and `json.loads()` do, using the album example from the chapter.

> [!success]- Answer **json.dumps()** - Encodes a Python object to a JSON string
> 
> ```python
> import json
> album = {'id': 42, 'title': "Back in Black"}
> string = json.dumps(album)  # Converts Python dict to JSON string
> ```
> 
> **json.loads()** - Decodes a JSON string to a Python object
> 
> ```python
> album = json.loads(string)  # Converts JSON string back to Python dict
> ```
> 
> These functions allow conversion between Python objects and JSON strings for data exchange.

**Q50.** In the lyric API example, what was the difference in output when requesting without headers versus with the `accept: application/json` header?

> [!success]- Answer **Without headers:**
> 
> ```
> N' I never miss Cause I'm a problem child - AC/DC, Problem Child
> ```
> 
> The response was plain text.
> 
> **With accept header:**
> 
> ```python
> headers={'accept': 'application/json'}
> # Response:
> {'artist': 'AC/DC', 'lyric': "N' I never miss Cause I'm a problem child", 'track': 'Problem Child'}
> ```
> 
> The response was structured JSON that could be parsed with `response.json()` and accessed like `data['artist']` to get `'AC/DC'`.

**Q51.** Explain the difference between 4xx and 5xx error status codes. Who should fix each type?

> [!success]- Answer **4xx Client Errors:**
> 
> - Indicate issues on the client's end
> - Common causes: Bad requests, authentication failures, etc.
> - **Resolution: Fix the request** (client's responsibility)
> 
> **5xx Server Errors:**
> 
> - Arise from problems on the server
> - Common causes: Server overloaded, server configuration errors, internal errors
> - **Resolution: Should be fixed by the API administrator** (server's responsibility)
> 
> In summary: 4xx = client must fix, 5xx = server must fix.

**Q52.** List and describe the three specific 4xx error codes shown in the chapter examples.

> [!success]- Answer **401 Unauthorized**
> 
> - The request lacks valid authentication credentials for the requested resource
> - User needs to provide proper authentication
> 
> **404 Not Found**
> 
> - Indicates that the server cannot find the resource that was requested
> - The URL or endpoint doesn't exist
> 
> **429 Too Many Requests**
> 
> - The client has sent too many requests in a given amount of time
> - Rate limit has been exceeded

**Q53.** List and describe the three specific 5xx error codes shown in the chapter examples.

> [!success]- Answer **500 Internal Server Error**
> 
> - The server experienced an unexpected issue which prevents it from responding
> - Generic server-side error
> 
> **502 Bad Gateway**
> 
> - The API server could not successfully reach another server it needed to complete the response
> - Problem with upstream server communication
> 
> **504 Gateway Timeout**
> 
> - The server (which acts as a gateway) did not get a response from the upstream server in time
> - Timeout issue with upstream server

**Q54.** Describe the error handling workflow shown in the chapter using raise_for_status(). Include the imports and exception types.

> [!success]- Answer **Complete error handling workflow:**
> 
> ```python
> import requests
> # 1: Import the requests library exceptions
> from requests.exceptions import ConnectionError, HTTPError
> 
> try:
>     r = requests.get("http://api.music-catalog.com/albums")
>     # 2: Enable raising exceptions for returned error statuscodes
>     r.raise_for_status()
>     print(r.status_code)
>     # 3: Catch any connection errors
> except ConnectionError as conn_err:
>     print(f'Connection Error! {conn_err}.')
>     # 4: Catch error responses from the API server
> except HTTPError as http_err:
>     print(f'HTTP error occurred: {http_err}')
> ```
> 
> **Steps:**
> 
> 1. Import ConnectionError and HTTPError exceptions
> 2. Call raise_for_status() to enable automatic error raising
> 3. Catch ConnectionError for network/connection issues
> 4. Catch HTTPError for API error responses (4xx/5xx)

**Q55.** What is the content-type header value when using the `json=` argument in requests.post()? Show the example from the chapter.

> [!success]- Answer When using the `json=` argument, the content-type header is automatically set to **`application/json`**.
> 
> **Chapter example:**
> 
> ```python
> import requests
> playlist = {"name": "Road trip", "genre":"rock", "private":"true"}
> 
> # Add the playlist using via the `json` argument
> response = requests.post("http://api.music-catalog.com/playlists", json=playlist)
> 
> # Get the request object
> request = response.request
> 
> # Print the request content-type header
> print(request.headers['content-type'])
> ```
> 
> **Output:** `application/json`
> 
> This shows that the `json=` parameter automatically sets the proper content-type header.

---

## Section D: Code Analysis & Scenarios (25 points)

_2.5 points each | 10 questions total_

**Q56.** What does this code from the chapter do? Explain each part.

```python
requests.get('http://api.music-catalog.com', auth=('username', 'password'))
```

> [!success]- Answer **This code performs Basic Authentication.**
> 
> **Explanation:**
> 
> - `requests.get()` - Makes a GET request
> - `'http://api.music-catalog.com'` - The API endpoint URL
> - `auth=('username', 'password')` - Provides Basic Authentication credentials
> 
> **What happens:** The `auth` parameter automatically adds a Basic Authentication header to the request before sending it. The username and password are encoded and sent with the request to authenticate with the API.

**Q57.** Complete this code to pass an API token using a query parameter:

```python
params = {'access_token': __________}
response = requests.get('http://api.music-catalog.com/albums', __________)
```

> [!success]- Answer **Completed code:**
> 
> ```python
> params = {'access_token': 'faaa1c97bd3f4bd9b024c708c979feca'}
> response = requests.get('http://api.music-catalog.com/albums', params=params)
> ```
> 
> This passes the API token as a query parameter, creating a URL like: `http://api.music-catalog.com/albums?access_token=faaa1c97bd3f4bd9b024c708c979feca`

**Q58.** Complete this code to pass an API token using the Bearer authorization header:

```python
headers = {'Authorization': __________}
response = requests.get('http://api.music-catalog.com/albums', __________)
```

> [!success]- Answer **Completed code:**
> 
> ```python
> headers = {'Authorization': 'Bearer faaa1c97bd3f4bd9b024c708c979feca'}
> response = requests.get('http://api.music-catalog.com/albums', headers=headers)
> ```
> 
> This passes the API token in the Authorization header using the Bearer scheme, which is more secure than query parameters.

**Q59.** What does this code snippet from the chapter do?

```python
import json
album = {'id': 42, 'title':"Back in Black"}
string = json.dumps(album)
```

> [!success]- Answer **This code encodes a Python object to a JSON string.**
> 
> **Step by step:**
> 
> 1. Import the json module
> 2. Create a Python dictionary called `album` with 'id' and 'title' keys
> 3. Use `json.dumps(album)` to convert the Python dictionary into a JSON-formatted string
> 
> **Result:** The `string` variable contains: `'{"id": 42, "title": "Back in Black"}'`
> 
> This is useful for sending Python data to APIs that expect JSON format.

**Q60.** Complete this code to request JSON-formatted data and access the artist field:

```python
response = requests.get('http://api.music-catalog.com/lyrics', headers={'accept': __________})
data = response.__________()
print(data[__________])
```

> [!success]- Answer **Completed code:**
> 
> ```python
> response = requests.get('http://api.music-catalog.com/lyrics', headers={'accept': 'application/json'})
> data = response.json()
> print(data['artist'])
> ```
> 
> **Explanation:**
> 
> - First blank: `'application/json'` - Requests JSON format via accept header
> - Second blank: `json` - Calls response.json() to decode the JSON
> - Third blank: `'artist'` - Accesses the artist field from the dictionary
> 
> **Output:** `AC/DC`

**Q61.** Complete this code to send JSON data to create a playlist:

```python
import requests
playlist = {"name": "Road trip", "genre":"rock", "private":"true"}
response = requests.post("http://api.music-catalog.com/playlists", __________)
```

> [!success]- Answer **Completed code:**
> 
> ```python
> import requests
> playlist = {"name": "Road trip", "genre":"rock", "private":"true"}
> response = requests.post("http://api.music-catalog.com/playlists", json=playlist)
> ```
> 
> Using the `json=` argument automatically:
> 
> - Converts the Python dictionary to JSON format
> - Sets the Content-Type header to `application/json`
> - Sends the JSON data in the request body

**Q62.** What's wrong with this error checking code? How should it be fixed based on the chapter?

```python
response = requests.get(url)
if response.status_code == 400:
    print("Error occurred")
else:
    data = response.json()
```

> [!success]- Answer **Problem:** This only checks for status code 400, missing all other error codes (401, 404, 429, 500, 502, 504, etc.).
> 
> **Fixed version (based on chapter approach):**
> 
> ```python
> response = requests.get(url)
> if response.status_code >= 400:
>     # Oops, something went wrong
>     print("Error occurred")
> else:
>     # All fine, let's do something with the response
>     data = response.json()
> ```
> 
> The chapter shows checking `if r.status_code >= 400:` to catch ALL error status codes (both 4xx client errors and 5xx server errors).

**Q63.** Complete this error handling code using the pattern from the chapter:

```python
from requests.exceptions import ConnectionError
url = 'http://api.music-catalog.com/albums'
try:
    r = requests.get(url)
    print(r.status_code)
except __________ as conn_err:
    print(f'Connection Error! {conn_err}.')
```

> [!success]- Answer **Completed code:**
> 
> ```python
> from requests.exceptions import ConnectionError
> url = 'http://api.music-catalog.com/albums'
> try:
>     r = requests.get(url)
>     print(r.status_code)
> except ConnectionError as conn_err:
>     print(f'Connection Error! {conn_err}.')
> ```
> 
> The blank should be filled with **`ConnectionError`** to catch connection errors as shown in the chapter's error handling example.

**Q64.** Complete this code using raise_for_status() following the chapter's pattern:

```python
import requests
from requests.exceptions import ConnectionError, HTTPError

try:
    r = requests.get("http://api.music-catalog.com/albums")
    r.__________()
    print(r.status_code)
except ConnectionError as conn_err:
    print(f'Connection Error! {conn_err}.')
except __________ as http_err:
    print(f'HTTP error occurred: {http_err}')
```

> [!success]- Answer **Completed code:**
> 
> ```python
> import requests
> from requests.exceptions import ConnectionError, HTTPError
> ```

try: r = requests.get("http://api.music-catalog.com/albums") r.raise_for_status() print(r.status_code) except ConnectionError as conn_err: print(f'Connection Error! {conn_err}.') except HTTPError as http_err: print(f'HTTP error occurred: {http_err}')

````
> 
> **Blanks filled:**
> - First blank: `raise_for_status` - Enables raising exceptions for error status codes
> - Second blank: `HTTPError` - Catches error responses from the API server

**Q65.** Based on the chapter examples, if you receive a 502 Bad Gateway error, is this a client error or server error? Who should fix it?

> [!success]- Answer
> **502 Bad Gateway is a server error (5xx).**
> 
> **From the chapter:**
> - "502 Bad Gateway - The API server could not successfully reach another server it needed to complete the response"
> - Listed under "5xx Server Errors"
> 
> **Who should fix it:**
> The API administrator should fix it. The chapter states: "Resolution: Should be fixed by the API administrator" for 5xx server errors.
> 
> **This is NOT the client's fault** - it's a problem on the server side where the API server couldn't reach another server it needed.

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)
1. B | 2. C | 3. D | 4. B | 5. B | 6. C | 7. B | 8. B | 9. A | 10. B
2. B | 12. D | 13. A | 14. B | 15. B | 16. B | 17. B | 18. C | 19. B | 20. C
3. B | 22. B | 23. C | 24. C | 25. C | 26. B | 27. B | 28. B | 29. C | 30. C

### Section B: True/False (15 points)
31. T | 32. T | 33. F | 34. T | 35. T | 36. F | 37. F | 38. T | 39. T | 40. F
32. T | 42. T | 43. F | 44. T | 45. F

### Section C: Short Answer (30 points)
- Questions 46-55 require detailed written answers
- Full credit requires complete information matching the chapter content
- Partial credit for incomplete but correct information

### Section D: Code Analysis (25 points)
- Questions 56-65 require code completion or analysis
- Full credit requires correct answers matching chapter examples
- Partial credit for partially correct solutions

---

## Quick Study Guide

### Authentication Methods (From Chapter Table)
| Method | Ease | Security |
|--------|------|----------|
| Basic | ⭐⭐⭐⭐ | ⭐ |
| API Key/Token | ⭐⭐⭐ | ⭐⭐⭐ |
| JWT | ⭐⭐ | ⭐⭐⭐⭐ |
| OAuth 2.0 | ⭐ | ⭐⭐⭐⭐⭐ |

### Authentication Code Patterns
```python
# Basic Authentication
requests.get(url, auth=('username', 'password'))

# API Key - Query Parameter
params = {'access_token': 'token'}
requests.get(url, params=params)

# API Key - Bearer Header
headers = {'Authorization': 'Bearer token'}
requests.get(url, headers=headers)
````

### JSON Operations

```python
import json

# Encode: Python → JSON string
string = json.dumps(album)

# Decode: JSON string → Python
album = json.loads(string)

# Response to Python (automatic)
data = response.json()
```

### Sending JSON Data

```python
playlist = {"name": "Road trip", "genre": "rock"}
response = requests.post(url, json=playlist)

# This automatically sets:
# Content-Type: application/json
```

### Error Status Codes

**4xx - Client Errors:**

- 401 Unauthorized - Missing/invalid authentication
- 404 Not Found - Resource doesn't exist
- 429 Too Many Requests - Rate limit exceeded

**5xx - Server Errors:**

- 500 Internal Server Error - Unexpected server issue
- 502 Bad Gateway - Couldn't reach upstream server
- 504 Gateway Timeout - Upstream server timeout

### Error Handling Pattern

```python
from requests.exceptions import ConnectionError, HTTPError

try:
    r = requests.get(url)
    r.raise_for_status()
    print(r.status_code)
except ConnectionError as conn_err:
    print(f'Connection Error! {conn_err}.')
except HTTPError as http_err:
    print(f'HTTP error occurred: {http_err}')
```

---

## Key Concepts Summary

### JSON Characteristics

✅ Widely supported  
✅ Human readable  
✅ Machine usable

### Other Formats Mentioned

- XML
- CSV
- YAML

### Content Type

- Use `accept: application/json` header to request JSON format
- The `json=` parameter automatically sets `Content-Type: application/json`

### Error Resolution

- **4xx errors**: Client must fix the request
- **5xx errors**: API administrator must fix

---

## Common Mistakes to Avoid

❌ **Forgetting to import json module**

```python
# Need to import if using dumps/loads
import json
```

❌ **Not using auth parameter for Basic Authentication**

```python
# WRONG - manual
# CORRECT - automatic
requests.get(url, auth=('user', 'pass'))
```

❌ **Checking only specific status codes**

```python
# WRONG - misses other errors
if response.status_code == 404:
    
# CORRECT - catches all errors
if response.status_code >= 400:
```

❌ **Not catching ConnectionError**

```python
# WRONG - no connection error handling
try:
    r = requests.get(url)
    
# CORRECT - handle connection issues
except ConnectionError:
    print("Connection failed")
```

---

## Study Strategy (5-Day Plan)

### Day 1: Authentication

- [ ] Four authentication methods and ratings
- [ ] Basic Authentication with auth parameter
- [ ] Two methods for API keys (query param & Bearer header)
- [ ] Check API documentation for method
- **Practice**: Implement both API key methods

### Day 2: JSON Basics

- [ ] What is JSON - three characteristics
- [ ] json.dumps() - Python to JSON
- [ ] json.loads() - JSON to Python
- [ ] Other formats (XML, CSV, YAML)
- **Practice**: Convert between Python and JSON

### Day 3: Requesting & Sending JSON

- [ ] Use accept header to request JSON
- [ ] response.json() to decode
- [ ] json= parameter to send JSON
- [ ] Automatic Content-Type header
- **Practice**: Request and send JSON data

### Day 4: Error Status Codes

- [ ] 4xx = client errors (fix request)
- [ ] 5xx = server errors (admin fixes)
- [ ] Specific codes: 401, 404, 429, 500, 502, 504
- [ ] Check if status_code >= 400
- **Practice**: Identify error types

### Day 5: Error Handling & Review

- [ ] Import ConnectionError and HTTPError
- [ ] Use raise_for_status()
- [ ] try-except pattern
- [ ] Complete examples from chapter
- **Practice**: Write complete error handling, take quiz

---

## Final Exam Checklist

### Authentication ✓

- [ ] Know the four methods and their ease/security ratings
- [ ] Can write Basic Authentication code
- [ ] Know both API key methods (query & header)
- [ ] Remember to check API documentation

### JSON ✓

- [ ] Know JSON characteristics
- [ ] Understand dumps vs loads
- [ ] Can use response.json()
- [ ] Know when to use json= parameter

### Error Codes ✓

- [ ] Know 4xx = client, 5xx = server
- [ ] Memorize specific codes from chapter
- [ ] Know who fixes each type

### Error Handling ✓

- [ ] Know imports needed
- [ ] Can use raise_for_status()
- [ ] Understand try-except structure
- [ ] Can write complete error handling

---

**Good luck! 🎯**

Remember: This quiz covers **exactly** what was in Chapter 2 - no more, no less!

---

#datacamp #apis #python #authentication #json #error-handling #certification #chapter2