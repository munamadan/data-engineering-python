
## Section A: Multiple Choice Questions (60 points)

_2 points each | 30 questions total_

### Web Data & HTTP (Questions 1-10)

**Q1.** What does URL stand for?

- [ ] A) Universal Resource Link
- [ ] B) Uniform Resource Locator
- [ ] C) Universal Reference Locator
- [ ] D) Uniform Reference Link

> [!success]- Answer **Correct Answer: B) Uniform Resource Locator**
> 
> URL is a reference to web resources consisting of a protocol identifier (http:) and resource name (datacamp.com).

**Q2.** What does HTTP stand for?

- [ ] A) HyperText Transfer Protocol
- [ ] B) HyperText Transmission Protocol
- [ ] C) HighText Transfer Protocol
- [ ] D) HyperText Transport Protocol

> [!success]- Answer **Correct Answer: A) HyperText Transfer Protocol**
> 
> HTTP is the foundation of data communication for the web. HTTPS is its more secure form.

**Q3.** Which function downloads a file from a URL and saves it locally?

- [ ] A) urlopen()
- [ ] B) urlretrieve()
- [ ] C) download()
- [ ] D) get_file()

> [!success]- Answer **Correct Answer: B) urlretrieve()**
> 
> ```python
> from urllib.request import urlretrieve
> urlretrieve(url, 'filename.csv')
> ```

**Q4.** What package is one of the most downloaded Python packages for HTTP?

- [ ] A) urllib
- [ ] B) pandas
- [ ] C) requests
- [ ] D) numpy

> [!success]- Answer **Correct Answer: C) requests**
> 
> Used by Google, Amazon, Twitter, and government institutions. Much simpler than urllib.

**Q5.** What does BeautifulSoup do?

- [ ] A) Downloads files
- [ ] B) Parses and extracts data from HTML
- [ ] C) Creates websites
- [ ] D) Compresses data

> [!success]- Answer **Correct Answer: B) Parses and extracts data from HTML**
> 
> BeautifulSoup (from bs4) makes 'tag soup' beautiful and extracts structured information from HTML.

**Q6.** What type of HTTP request retrieves data?

- [ ] A) POST
- [ ] B) PUT
- [ ] C) GET
- [ ] D) DELETE

> [!success]- Answer **Correct Answer: C) GET**
> 
> GET requests retrieve data from servers. urlretrieve() and requests.get() both perform GET requests.

**Q7.** Which method extracts all text from HTML in BeautifulSoup?

- [ ] A) .text()
- [ ] B) .get_text()
- [ ] C) .extract()
- [ ] D) .read_text()

> [!success]- Answer **Correct Answer: B) .get_text()**
> 
> ```python
> soup.get_text()  # Removes all HTML tags, returns plain text
> ```

**Q8.** What does `.find_all('a')` find in BeautifulSoup?

- [ ] A) All paragraphs
- [ ] B) All links (anchor tags)
- [ ] C) All headers
- [ ] D) All images

> [!success]- Answer **Correct Answer: B) All links (anchor tags)**
> 
> ```python
> for link in soup.find_all('a'):
>     print(link.get('href'))
> ```

**Q9.** Which package is built into Python (no installation)?

- [ ] A) requests
- [ ] B) beautifulsoup4
- [ ] C) urllib
- [ ] D) scrapy

> [!success]- Answer **Correct Answer: C) urllib**
> 
> urllib is part of Python's standard library. requests and BeautifulSoup require pip installation.

**Q10.** What attribute contains the URL in an anchor tag?

- [ ] A) url
- [ ] B) link
- [ ] C) href
- [ ] D) src

> [!success]- Answer **Correct Answer: C) href**
> 
> ```python
> link.get('href')  # Extract URL from <a> tag
> ```

### APIs & JSON (Questions 11-20)

**Q11.** What does API stand for?

- [ ] A) Application Program Interface
- [ ] B) Application Programming Interface
- [ ] C) Applied Programming Interface
- [ ] D) Application Process Interface

> [!success]- Answer **Correct Answer: B) Application Programming Interface**
> 
> APIs are protocols and routines allowing two software programs to communicate.

**Q12.** What does JSON stand for?

- [ ] A) JavaScript Object Notation
- [ ] B) Java Script Object Notation
- [ ] C) JavaScript Online Notation
- [ ] D) Java Object Notation

> [!success]- Answer **Correct Answer: A) JavaScript Object Notation**
> 
> Created by Douglas Crockford for server-to-browser communication. Human-readable format.

**Q13.** Who created JSON?

- [ ] A) Brendan Eich
- [ ] B) Douglas Crockford
- [ ] C) Guido van Rossum
- [ ] D) Tim Berners-Lee

> [!success]- Answer **Correct Answer: B) Douglas Crockford**
> 
> Douglas Crockford specified the JSON format for real-time data communication.

**Q14.** What Python type does JSON load as?

- [ ] A) list
- [ ] B) tuple
- [ ] C) dictionary
- [ ] D) string

> [!success]- Answer **Correct Answer: C) dictionary**
> 
> ```python
> import json
> with open('file.json', 'r') as f:
>     data = json.load(f)
> type(data)  # <class 'dict'>
> ```

**Q15.** Which function loads JSON from a file?

- [ ] A) json.read()
- [ ] B) json.load()
- [ ] C) json.open()
- [ ] D) json.get()

> [!success]- Answer **Correct Answer: B) json.load()**
> 
> json.load() for files, json.loads() for strings.

**Q16.** What method converts API response to JSON?

- [ ] A) .to_json()
- [ ] B) .get_json()
- [ ] C) .json()
- [ ] D) .parse()

> [!success]- Answer **Correct Answer: C) .json()**
> 
> ```python
> import requests
> r = requests.get(url)
> data = r.json()  # Convert to dictionary
> ```

**Q17.** What character starts a query string in a URL?

- [ ] A) &
- [ ] B) #
- [ ] C) ?
- [ ] D) /

> [!success]- Answer **Correct Answer: C) ?**
> 
> Format: `http://api.com/endpoint?param1=value1&param2=value2`

**Q18.** What character separates multiple parameters in a URL?

- [ ] A) ?
- [ ] B) &
- [ ] C) ,
- [ ] D) ;

> [!success]- Answer **Correct Answer: B) &**
> 
> Example: `?title=movie&year=2020&genre=action`

**Q19.** In `?t=hackers`, what does `t` represent?

- [ ] A) Token
- [ ] B) Type
- [ ] C) Parameter name (title)
- [ ] D) Tag

> [!success]- Answer **Correct Answer: C) Parameter name (title)**
> 
> Query format: `param=value`. Here 't' is the parameter for title.

**Q20.** Is JSON human readable?

- [ ] A) No
- [ ] B) Yes
- [ ] C) Only for programmers
- [ ] D) Only in browsers

> [!success]- Answer **Correct Answer: B) Yes**
> 
> JSON is designed to be human-readable with clear key-value structure.

### Twitter API & Authentication (Questions 21-30)

**Q21.** Which package is used for Twitter API in Python?

- [ ] A) twitter-python
- [ ] B) tweepy
- [ ] C) python-twitter
- [ ] D) twitterapi

> [!success]- Answer **Correct Answer: B) tweepy**
> 
> ```python
> import tweepy
> stream = tweepy.Stream(consumer_key, consumer_secret,
>                        access_token, access_token_secret)
> ```

**Q22.** What format does Twitter API return tweets in?

- [ ] A) XML
- [ ] B) CSV
- [ ] C) JSON
- [ ] D) HTML

> [!success]- Answer **Correct Answer: C) JSON**
> 
> Twitter API returns all tweet data as JSON objects.

**Q23.** How many authentication credentials are needed for Twitter API?

- [ ] A) 2
- [ ] B) 3
- [ ] C) 4
- [ ] D) 5

> [!success]- Answer **Correct Answer: C) 4**
> 
> - access_token
> - access_token_secret
> - consumer_key
> - consumer_secret

**Q24.** What authentication protocol does Twitter use?

- [ ] A) Basic Auth
- [ ] B) API Key only
- [ ] C) OAuth
- [ ] D) JWT

> [!success]- Answer **Correct Answer: C) OAuth**
> 
> Industry-standard authorization protocol for secure API access.

**Q25.** Which parameter filters Twitter streams by keywords?

- [ ] A) filter
- [ ] B) keywords
- [ ] C) track
- [ ] D) search

> [!success]- Answer **Correct Answer: C) track**
> 
> ```python
> stream.filter(track=['python', 'data'])
> ```

**Q26.** What does HTTPS provide over HTTP?

- [ ] A) Faster speed
- [ ] B) More security
- [ ] C) More data
- [ ] D) Better images

> [!success]- Answer **Correct Answer: B) More security**
> 
> HTTPS is the secure version of HTTP with encryption.

**Q27.** What does `.prettify()` do in BeautifulSoup?

- [ ] A) Downloads HTML
- [ ] B) Formats HTML for readability
- [ ] C) Deletes HTML
- [ ] D) Compresses HTML

> [!success]- Answer **Correct Answer: B) Formats HTML for readability**
> 
> Adds indentation to show HTML structure clearly.

**Q28.** What does HTML stand for?

- [ ] A) HyperText Markup Language
- [ ] B) HighText Markup Language
- [ ] C) HyperText Making Language
- [ ] D) HyperText Multi Language

> [!success]- Answer **Correct Answer: A) HyperText Markup Language**
> 
> Standard markup language for web pages.

**Q29.** Which is simpler for GET requests?

- [ ] A) urllib
- [ ] B) requests
- [ ] C) Both equal
- [ ] D) Neither

> [!success]- Answer **Correct Answer: B) requests**
> 
> requests: `r = requests.get(url); text = r.text`
> 
> urllib: requires Request, urlopen, read(), close()

**Q30.** Why automate web data imports?

- [ ] A) It's fun
- [ ] B) Reproducibility and scalability
- [ ] C) Required by law
- [ ] D) Faster internet

> [!success]- Answer **Correct Answer: B) Reproducibility and scalability**
> 
> Manual downloads aren't reproducible (others can't repeat) or scalable (can't handle hundreds of files).

---

## Section B: True/False Questions (15 points)

_1 point each | 15 questions total_

**Q31.** URL stands for Uniform Resource Locator. **T/F**

> [!success]- Answer **True** - References to web resources consisting of protocol identifier and resource name.

**Q32.** HTTP is the foundation of web data communication. **T/F**

> [!success]- Answer **True** - HyperText Transfer Protocol is the foundation of data communication for the web.

**Q33.** urlretrieve() requires the requests package. **T/F**

> [!success]- Answer **False** - It's part of urllib.request which is built into Python's standard library.

**Q34.** requests.get() is simpler than urllib for GET requests. **T/F**

> [!success]- Answer **True** - requests has much more user-friendly syntax compared to urllib.

**Q35.** BeautifulSoup can parse HTML. **T/F**

> [!success]- Answer **True** - BeautifulSoup extracts structured data from HTML documents.

**Q36.** Manual file downloads are reproducible and scalable. **T/F**

> [!success]- Answer **False** - Manual downloads are neither reproducible (others can't repeat exact steps) nor scalable (can't handle hundreds of files efficiently).

**Q37.** HTTPS is more secure than HTTP. **T/F**

> [!success]- Answer **True** - HTTPS adds an encryption layer for security.

**Q38.** `.find_all()` finds all instances of an HTML tag. **T/F**

> [!success]- Answer **True** - BeautifulSoup's `.find_all()` method finds all occurrences of a specified HTML tag.

**Q39.** urllib requires installation via pip. **T/F**

> [!success]- Answer **False** - urllib is built into Python's standard library and requires no installation.

**Q40.** Structured data has a pre-defined data model. **T/F**

> [!success]- Answer **True** - Structured data is organized in a defined manner with a pre-defined data model.

**Q41.** API stands for Application Programming Interface. **T/F**

> [!success]- Answer **True** - APIs are protocols and routines that allow software programs to communicate.

**Q42.** JSON is only used with JavaScript. **T/F**

> [!success]- Answer **False** - JSON is language-independent and used in Python, Java, and many other languages.

**Q43.** JSON loads as a Python dictionary. **T/F**

> [!success]- Answer **True** - json.load() returns data as a Python dictionary type.

**Q44.** Query strings start with `?` in URLs. **T/F**

> [!success]- Answer **True** - Query strings in URLs begin with `?` followed by parameter=value pairs.

**Q45.** Tweepy requires all 4 authentication credentials. **T/F**

> [!success]- Answer **True** - Tweepy requires consumer_key, consumer_secret, access_token, and access_token_secret for OAuth authentication.

---

## Section C: Short Answer Questions (30 points)

_3 points each | 10 questions total_

**Q46.** Explain the difference between urllib and requests for making HTTP requests.

> [!success]- Answer **urllib:**
> 
> - Built into Python (no installation required)
> - More complex syntax
> - Requires Request object, urlopen(), read(), and close()
> - More verbose code
> 
> **requests:**
> 
> - Requires pip installation
> - Simpler, more user-friendly syntax
> - One-line GET request: `r = requests.get(url)`
> - Used by major organizations (Google, Amazon, Twitter)
> - **Recommended for most use cases**
> 
> **Example comparison:**
> 
> ```python
> # requests - simple
> r = requests.get(url)
> text = r.text
> 
> # urllib - complex
> from urllib.request import Request, urlopen
> request = Request(url)
> response = urlopen(request)
> html = response.read()
> response.close()
> ```

**Q47.** What are the key components of a URL?

> [!success]- Answer A URL consists of four main components:
> 
> 1. **Protocol identifier:** `http:` or `https:`
>     - Specifies how data is transferred
> 2. **Domain/Resource name:** `www.example.com`
>     - Identifies the server location
> 3. **Path:** `/api/data`
>     - Specifies the resource location on the server
> 4. **Query string (optional):** `?param=value&param2=value2`
>     - Passes parameters to customize the request
> 
> **Complete example:** `http://api.example.com/endpoint?key=value&type=json`

**Q48.** List the four Twitter API authentication credentials and their purposes.

> [!success]- Answer Twitter API requires four OAuth authentication credentials:
> 
> 1. **consumer_key**
>     - Identifies the application
>     - Public identifier for your app
> 2. **consumer_secret**
>     - Secret key for the application
>     - Must be kept confidential
> 3. **access_token**
>     - User-specific authentication token
>     - Identifies the user account
> 4. **access_token_secret**
>     - Secret token for user authentication
>     - Must be kept confidential
> 
> All four are required for OAuth authentication when using Tweepy to access the Twitter API.

**Q49.** What are three methods BeautifulSoup provides for parsing HTML?

> [!success]- Answer BeautifulSoup provides several key methods for HTML parsing:
> 
> 1. **`.get_text()`**
>     - Extracts all text content from HTML
>     - Removes all HTML tags
>     - Returns plain text only
> 2. **`.find_all('tag')`**
>     - Finds all occurrences of a specific HTML tag
>     - Returns a list of matching elements
>     - Example: `soup.find_all('a')` finds all links
> 3. **`.prettify()`**
>     - Formats HTML with proper indentation
>     - Makes HTML structure more readable
>     - Useful for debugging
> 
> **Additional useful methods:**
> 
> - `.find()` - Finds first occurrence
> - `.select()` - CSS selector
> - `.get('attribute')` - Extracts attribute values

**Q50.** Explain what JSON is and why it's useful for APIs.

> [!success]- Answer **JSON (JavaScript Object Notation):**
> 
> - Human-readable data format
> - Created by Douglas Crockford
> - Uses key-value pair structure
> - Language-independent (works with any programming language)
> 
> **Structure example:**
> 
> ```json
> {
>   "name": "John",
>   "age": 30,
>   "city": "New York"
> }
> ```
> 
> **Why useful for APIs:**
> 
> 1. **Easy to parse** - Simple syntax in any language
> 2. **Lightweight** - Minimal overhead for data transmission
> 3. **Supports nested structures** - Can represent complex data
> 4. **Human readable** - Easy to understand and debug
> 5. **Standard format** - Universal adoption for web APIs
> 6. **Direct mapping** - Converts easily to Python dictionaries
> 
> JSON has become the de facto standard for API data exchange.

**Q51.** What is the purpose of query strings in URLs?

> [!success]- Answer Query strings pass parameters to APIs/servers to customize requests and filter results.
> 
> **Format:** `?param1=value1&param2=value2`
> 
> **Components:**
> 
> - Starts with `?` (question mark)
> - Parameters separated by `&` (ampersand)
> - Each parameter: `name=value`
> 
> **Example:**
> 
> ```
> http://api.weather.com?city=london&units=metric&lang=en
> ```
> 
> **Purposes:**
> 
> 1. **Filtering:** Narrow down results
> 2. **Searching:** Specify search terms
> 3. **Customization:** Set preferences (language, format)
> 4. **Pagination:** Specify page number and size
> 5. **Sorting:** Define sort order
> 
> Query strings allow flexible API interactions without changing the base URL.

**Q52.** Why is reproducibility important in web data collection?

> [!success]- Answer **Reproducibility ensures:**
> 
> 1. **Others can repeat your exact steps**
>     - Documented process in code
>     - No ambiguity in methodology
> 2. **Results can be verified**
>     - Independent validation possible
>     - Increases scientific credibility
> 3. **Process can be automated**
>     - Run repeatedly without manual intervention
>     - Schedule regular data updates
> 4. **Documentation of data sources**
>     - URLs and parameters in code
>     - Clear provenance trail
> 
> **Manual downloads are NOT reproducible:**
> 
> - Click-and-save steps can't be precisely repeated
> - No record of exact source
> - Human error prone
> - Not scalable for multiple files
> 
> **Automated Python scripts ensure:**
> 
> - Complete reproducibility
> - Scalability (handle hundreds of files)
> - Consistency across runs
> - Audit trail

**Q53.** What is the difference between structured and unstructured data?

> [!success]- Answer **Structured Data:**
> 
> - Has a pre-defined data model
> - Organized in a defined manner
> - Easily searchable
> - Examples: Tables, databases, CSV files, spreadsheets
> - Characteristics: Rows, columns, defined fields
> 
> **Unstructured Data:**
> 
> - No pre-defined data model
> - No defined organization
> - More difficult to search
> - Examples: Free text, images, videos, audio
> - Characteristics: Flexible, varied formats
> 
> **In HTML context:** HTML contains **both types:**
> 
> - Structured: Tables, lists, defined tags
> - Unstructured: Paragraph text, comments
> 
> **Key difference:** Structured data follows a schema; unstructured data doesn't.

**Q54.** How do you access data from a JSON API response using requests?

> [!success]- Answer **Complete workflow:**
> 
> ```python
> import requests
> 
> # Step 1: Make GET request
> r = requests.get('http://api.example.com/data')
> 
> # Step 2: Convert response to dictionary
> data = r.json()
> 
> # Step 3: Access specific values
> # Method 1: Direct key access
> value = data['key']
> 
> # Method 2: Safe access with .get()
> value = data.get('key', 'default')
> 
> # Step 4: Loop through data
> for key, value in data.items():
>     print(f"{key}: {value}")
> 
> # Step 5: Access nested data
> nested_value = data['parent']['child']
> 
> # Step 6: Handle lists in JSON
> for item in data['items']:
>     print(item['name'])
> ```
> 
> **Key points:**
> 
> - `.json()` automatically parses JSON to Python dict
> - Use dictionary methods to access data
> - Handle nested structures with chained keys
> - Use .get() for safe access to optional keys

**Q55.** What is OAuth and why is it used?

> [!success]- Answer **OAuth (Open Authorization):**
> 
> - Industry-standard authorization protocol
> - Provides secure API access without sharing passwords
> - Token-based authentication system
> - Allows delegated access with limited permissions
> 
> **How it works:**
> 
> 1. Application requests authorization
> 2. User grants permission
> 3. Application receives access token
> 4. Token used for API requests
> 
> **Why it's used:**
> 
> **Security benefits:**
> 
> - No password sharing required
> - Tokens can be revoked without changing password
> - Limited scope (permissions) per token
> - Tokens can expire automatically
> 
> **Used by major platforms:**
> 
> - Twitter
> - Google
> - Facebook
> - GitHub
> - LinkedIn
> 
> **Advantages:**
> 
> - Secure delegated access
> - Fine-grained permissions
> - Easy revocation
> - Industry standard
> - User control over access

---

## Section D: Code Analysis & Scenarios (25 points)

_2.5 points each | 10 questions total_

**Q56.** What does this code do?

```python
from urllib.request import urlretrieve
urlretrieve('http://example.com/data.csv', 'data.csv')
```

> [!success]- Answer **This code downloads a file from a URL and saves it locally.**
> 
> **Breakdown:**
> 
> - `from urllib.request import urlretrieve` - Imports the download function
> - First argument: `'http://example.com/data.csv'` - Source URL to download from
> - Second argument: `'data.csv'` - Local filename to save as
> - Performs GET request automatically
> - Returns tuple: `(filename, headers)`
> 
> **Use case:** Perfect for downloading datasets, images, or any file from the web to your local machine for processing.

**Q57.** What does this return?

```python
import requests
r = requests.get('http://example.com')
r.text
```

> [!success]- Answer **Returns the HTML/text content of the webpage as a string.**
> 
> **Explanation:**
> 
> - `requests.get()` makes HTTP GET request
> - `r` is the response object
> - `r.text` returns the response body as a Unicode string
> - Contains the full HTML source code of the webpage
> 
> **Alternative attributes:**
> 
> - `r.content` - Binary content (for images, PDFs)
> - `r.json()` - Parsed JSON (if response is JSON)
> - `r.status_code` - HTTP status code (200, 404, etc.)
> 
> **Example output:**
> 
> ```html
> '<!DOCTYPE html><html><head>...'
> ```

**Q58.** What does this code do?

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc)
soup.get_text()
```

> [!success]- Answer **Extracts all text content from HTML, removing all tags.**
> 
> **Step-by-step:**
> 
> 1. `from bs4 import BeautifulSoup` - Import parser
> 2. `soup = BeautifulSoup(html_doc)` - Create soup object from HTML
> 3. `soup.get_text()` - Extract plain text only
> 
> **What it removes:**
> 
> - All HTML tags (`<p>`, `<div>`, `<a>`, etc.)
> - Tag attributes
> - HTML structure
> 
> **What it keeps:**
> 
> - Pure text content
> - Whitespace (spaces, newlines)
> 
> **Use case:** Perfect for text analysis, word counting, or when you only need the textual content without HTML markup.
> 
> **Example:**
> 
> ```python
> # Input HTML
> '<p>Hello <b>World</b></p>'
> # Output
> 'Hello World'
> ```

**Q59.** What will this find?

```python
for link in soup.find_all('a'):
    print(link.get('href'))
```

> [!success]- Answer **Finds and prints all URLs from anchor tags (hyperlinks).**
> 
> **Breakdown:**
> 
> - `soup.find_all('a')` - Finds all `<a>` tags (links) in the HTML
> - Returns a list of all anchor tag elements
> - Loop iterates through each link
> - `link.get('href')` - Extracts the URL from the href attribute
> - Prints each URL on a separate line
> 
> **Example:**
> 
> ```html
> <!-- Input HTML -->
> <a href="http://example.com">Example</a>
> <a href="/about">About</a>
> 
> <!-- Output -->
> http://example.com
> /about
> ```
> 
> **Use case:** Web scraping to collect all links from a webpage, useful for building web crawlers or link analysis.

**Q60.** Complete the code to create a Twitter stream:

```python
import tweepy, json

stream = tweepy.________(consumer_key, consumer_secret,
                         access_token, access_token_secret)
```

> [!success]- Answer **Completed code:**
> 
> ```python
> import tweepy, json
> 
> stream = tweepy.Stream(consumer_key, consumer_secret,
>                        access_token, access_token_secret)
> ```
> 
> **Explanation:**
> 
> - `tweepy.Stream()` creates a streaming object
> - Requires all 4 OAuth credentials in order:
>     1. consumer_key
>     2. consumer_secret
>     3. access_token
>     4. access_token_secret
> 
> **Next step (typically):**
> 
> ```python
> # Filter by keywords
> stream.filter(track=['keyword1', 'keyword2'])
> ```
> 
> Creates a real-time stream for collecting tweets matching specified criteria.

**Q61.** Fix all issues in this code:

```python
import requests
r = requests.get('http://api.example.com/data')
data = r.text
print(data['title'])
```

> [!success]- Answer **Issue identified:** Trying to use dictionary indexing (`['title']`) on a string object (`r.text`).
> 
> **Fixed code:**
> 
> ```python
> import requests
> r = requests.get('http://api.example.com/data')
> data = r.json()  # Convert JSON response to dictionary
> print(data['title'])
> ```
> 
> **Explanation:**
> 
> - `r.text` returns a string (the raw response)
> - Strings don't support dictionary key access
> - `r.json()` parses the JSON and returns a Python dictionary
> - Now `data['title']` works correctly
> 
> **Remember:**
> 
> - `.text` → string
> - `.json()` → dictionary
> - Use `.json()` for API responses in JSON format

**Q62.** What's wrong with this code?

```python
import json
file = open('data.json', 'r')
data = json.load(file)
```

> [!success]- Answer **Issue: File is never closed (resource leak).**
> 
> **Problems:**
> 
> - File handle remains open
> - Can cause memory leaks
> - May prevent other programs from accessing the file
> - Not following Python best practices
> 
> **Fixed code:**
> 
> ```python
> import json
> with open('data.json', 'r') as file:
>     data = json.load(file)
> # File automatically closed after this block
> ```
> 
> **Why the fix works:**
> 
> - `with` statement creates a context manager
> - File automatically closes when block exits
> - Happens even if an error occurs
> - Pythonic and recommended approach
> 
> **Always remember:** Use context managers (`with` statement) for file operations!

**Q63.** Complete this code to filter tweets by keywords 'python' and 'data':

```python
stream._______(track=['python', 'data'])
```

> [!success]- Answer **Completed code:**
> 
> ```python
> stream.filter(track=['python', 'data'])
> ```
> 
> **Explanation:**
> 
> - `.filter()` method starts the filtered stream
> - `track` parameter specifies keywords to filter by
> - List format: `['keyword1', 'keyword2']`
> - Captures tweets containing **either** 'python' **or** 'data'
> - Real-time streaming of matching tweets
> 
> **Complete example:**
> 
> ```python
> import tweepy
> 
> stream = tweepy.Stream(consumer_key, consumer_secret,
>                        access_token, access_token_secret)
> stream.filter(track=['python', 'data'])
> ```

**Q64.** You need to extract all paragraph text from a webpage. Write the complete code.

> [!success]- Answer **Complete solution:**
> 
> ```python
> import requests
> from bs4 import BeautifulSoup
> 
> # Step 1: Get webpage
> r = requests.get('http://example.com')
> html_doc = r.text
> 
> # Step 2: Parse HTML with BeautifulSoup
> soup = BeautifulSoup(html_doc)
> 
> # Step 3: Extract all paragraphs
> for paragraph in soup.find_all('p'):
>     print(paragraph.get_text())
> ```
> 
> **Breakdown:**
> 
> 1. Import necessary libraries
> 2. Make GET request to fetch webpage
> 3. Get HTML content with `.text`
> 4. Create BeautifulSoup object to parse HTML
> 5. Use `.find_all('p')` to find all paragraph tags
> 6. Loop through paragraphs and extract text with `.get_text()`
> 
> **Alternative (store in list):**
> 
> ```python
> paragraphs = []
> for paragraph in soup.find_all('p'):
>     paragraphs.append(paragraph.get_text())
> ```

**Q65.** An API returns this URL structure: `http://api.movies.com/search?title=inception&year=2010`. What parameters are being passed?

> [!success]- Answer **Two parameters are being passed:**
> 
> 1. **`title=inception`**
>     - Parameter name: `title`
>     - Value: `inception`
>     - Searches for movie title
> 2. **`year=2010`**
>     - Parameter name: `year`
>     - Value: `2010`
>     - Filters by release year
> 
> **URL structure breakdown:**
> 
> ```
> http://api.movies.com/search?title=inception&year=2010
> └───────┬──────────┘└──┬──┘└────────┬────────┘└────┬────┘
>     Base URL        Path    Parameter 1   Parameter 2
> ```
> 
> - Query string starts with `?`
> - Parameters separated by `&`
> - Each parameter: `name=value`
> 
> **Result:** API will return movies with title "inception" released in 2010.
> 
> **Python equivalent:**
> 
> ```python
> params = {'title': 'inception', 'year': 2010}
> r = requests.get('http://api.movies.com/search', params=params)
> ```

---

## Answer Key & Scoring Guide

### Section A: Multiple Choice (60 points)

1. B | 2. A | 3. B | 4. C | 5. B | 6. C | 7. B | 8. B | 9. C | 10. C
2. B | 12. A | 13. B | 14. C | 15. B | 16. C | 17. C | 18. B | 19. C | 20. B
3. B | 22. C | 23. C | 24. C | 25. C | 26. B | 27. B | 28. A | 29. B | 30. B

### Section B: True/False (15 points)

31. T | 32. T | 33. F | 34. T | 35. T | 36. F | 37. T | 38. T | 39. F | 40. T
32. T | 42. F | 43. T | 44. T | 45. T

### Section C: Short Answer (30 points)

Questions 46-55 require detailed written answers. Award full points for complete, accurate responses with examples.

### Section D: Code Analysis (25 points)

Questions 56-65 require code analysis or completion. Award full points for correct, working solutions with explanations.

---

## Quick Study Guide

### Essential Code Patterns

**Web Downloading:**

```python
# Method 1: urlretrieve (download file)
from urllib.request import urlretrieve
urlretrieve(url, 'filename.csv')

# Method 2: urllib (GET request)
from urllib.request import urlopen, Request
request = Request(url)
response = urlopen(request)
html = response.read()
response.close()

# Method 3: requests (RECOMMENDED)
import requests
r = requests.get(url)
content = r.text
```

**HTML Parsing with BeautifulSoup:**

```python
from bs4 import BeautifulSoup
import requests

# Get and parse webpage
r = requests.get(url)
soup = BeautifulSoup(r.text)

# Common methods
soup.prettify()              # Format HTML nicely
soup.get_text()              # Extract all text
soup.title                   # Get title tag
soup.find_all('tag')         # Find all instances of tag
soup.find('tag')             # Find first instance
link.get('href')             # Get attribute value

# Example: Extract all links
for link in soup.find_all('a'):
    print(link.get('href'))

# Example: Extract all paragraphs
for p in soup.find_all('p'):
    print(p.get_text())
```

**JSON Handling:**

```python
# Load JSON from file
import json
with open('file.json', 'r') as f:
    data = json.load(f)

# Load JSON from string
json_string = '{"name": "John", "age": 30}'
data = json.loads(json_string)

# API request returning JSON
import requests
r = requests.get('http://api.example.com/data')
data = r.json()  # Automatically parse JSON

# Access JSON data
value = data['key']
value = data.get('key', 'default')

# Loop through JSON
for key, value in data.items():
    print(f"{key}: {value}")
```

**Twitter API with Tweepy:**

```python
import tweepy, json

# Authentication (4 credentials required)
consumer_key = "..."
consumer_secret = "..."
access_token = "..."
access_token_secret = "..."

# Create stream
stream = tweepy.Stream(consumer_key, consumer_secret,
                       access_token, access_token_secret)

# Filter by keywords
stream.filter(track=['keyword1', 'keyword2'])

# Filter by language
stream.filter(track=['python'], languages=['en'])
```

**URL Query Strings:**

```python
# Method 1: Manual (not recommended)
url = 'http://api.example.com?param1=value1&param2=value2'

# Method 2: Using params (RECOMMENDED)
params = {'param1': 'value1', 'param2': 'value2'}
r = requests.get('http://api.example.com', params=params)
```

---

## Key Concepts Summary

### Web Fundamentals

|Concept|Definition|Example|
|---|---|---|
|**URL**|Uniform Resource Locator|`http://example.com/path?key=value`|
|**HTTP**|HyperText Transfer Protocol|Foundation of web communication|
|**HTTPS**|Secure HTTP|Adds encryption|
|**GET**|Request type to retrieve data|Used by urllib, requests|
|**HTML**|HyperText Markup Language|Web page structure|

### Python Libraries

|Library|Purpose|Installation|Complexity|
|---|---|---|---|
|**urllib**|Basic web access|Built-in|High|
|**requests**|HTTP requests|`pip install requests`|Low|
|**BeautifulSoup**|HTML parsing|`pip install beautifulsoup4`|Medium|
|**json**|JSON handling|Built-in|Low|
|**tweepy**|Twitter API|`pip install tweepy`|Medium|

### Data Formats

|Format|Full Name|Use Case|Python Type|
|---|---|---|---|
|**JSON**|JavaScript Object Notation|APIs, config files|`dict`|
|**HTML**|HyperText Markup Language|Web pages|`string`|
|**CSV**|Comma Separated Values|Tabular data|`DataFrame`|
|**XML**|eXtensible Markup Language|Data exchange|`string`|

### HTTP Status Codes

|Code|Meaning|Description|
|---|---|---|
|200|OK|Request successful|
|404|Not Found|Resource doesn't exist|
|500|Internal Server Error|Server problem|

### BeautifulSoup Methods

|Method|Purpose|Returns|
|---|---|---|
|`.get_text()`|Extract text|String|
|`.find()`|Find first tag|Tag object|
|`.find_all()`|Find all tags|List of tags|
|`.prettify()`|Format HTML|String|
|`.get('attr')`|Get attribute|String|

### JSON Operations

|Operation|Function|Use|
|---|---|---|
|File → Dict|`json.load(f)`|Load from file|
|String → Dict|`json.loads(s)`|Parse JSON string|
|Dict → String|`json.dumps(d)`|Convert to JSON|
|API → Dict|`r.json()`|Parse API response|

---

## Common Mistakes to Avoid

❌ **Not using context managers for files**

```python
# WRONG - file never closed
file = open('data.json', 'r')
data = json.load(file)

# CORRECT - automatic closing
with open('data.json', 'r') as file:
    data = json.load(file)
```

❌ **Using .text instead of .json() for APIs**

```python
# WRONG - returns string
data = r.text
print(data['key'])  # Error!

# CORRECT - returns dict
data = r.json()
print(data['key'])  # Works!
```

❌ **Forgetting `?` before query string**

```python
# WRONG
url = 'http://api.com&param=value'

# CORRECT
url = 'http://api.com?param=value'
```

❌ **Using `&` to start query strings**

```python
# WRONG
url = 'http://api.com&key=value'

# CORRECT - starts with ?
url = 'http://api.com?key=value'

# Multiple parameters use &
url = 'http://api.com?key1=value1&key2=value2'
```

❌ **Not closing urllib responses**

```python
# WRONG
response = urlopen(request)
html = response.read()
# File handle left open

# CORRECT
response = urlopen(request)
html = response.read()
response.close()
```

❌ **Confusing json.load() and json.loads()**

```python
# json.load() - for FILES
with open('file.json') as f:
    data = json.load(f)

# json.loads() - for STRINGS
json_string = '{"key": "value"}'
data = json.loads(json_string)
```

❌ **Manual downloads (not reproducible)**

```python
# WRONG - manual, not reproducible
# Click download button, save file

# CORRECT - automated, reproducible
from urllib.request import urlretrieve
urlretrieve(url, 'data.csv')
```

❌ **Forgetting to import libraries**

```python
# WRONG
soup = BeautifulSoup(html)

# CORRECT
from bs4 import BeautifulSoup
soup = BeautifulSoup(html)
```

---

## Study Strategy (7-Day Plan)

### Day 1: Web Fundamentals

- [ ] URL structure and components
- [ ] HTTP vs HTTPS
- [ ] GET requests
- [ ] Understand reproducibility importance
- **Practice:** Make basic GET requests with urllib

### Day 2: requests Library

- [ ] Why requests > urllib
- [ ] requests.get() syntax
- [ ] .text vs .content vs .json()
- [ ] Basic error handling
- **Practice:** Download files with requests

### Day 3: HTML & BeautifulSoup

- [ ] HTML structure basics
- [ ] BeautifulSoup installation
- [ ] .get_text(), .find(), .find_all()
- [ ] Extracting links and text
- **Practice:** Parse real webpages

### Day 4: APIs Fundamentals

- [ ] What are APIs?
- [ ] API vs web scraping
- [ ] Query strings (? and &)
- [ ] Parameter passing
- **Practice:** Make API requests with parameters

### Day 5: JSON Handling

- [ ] JSON structure
- [ ] json.load() vs json.loads()
- [ ] Parsing API responses
- [ ] Accessing nested data
- **Practice:** Work with JSON files and APIs

### Day 6: Twitter API

- [ ] OAuth authentication
- [ ] 4 required credentials
- [ ] tweepy.Stream()
- [ ] Filtering tweets
- **Practice:** Set up Twitter API access

### Day 7: Review & Integration

- [ ] Complete workflows
- [ ] Debugging common errors
- [ ] Code optimization
- **Practice:** Take this practice quiz

---

## Final Exam Checklist

### Before the Exam ✓

**Web Basics:**

- [ ] Know URL components (protocol, domain, path, query)
- [ ] Understand HTTP vs HTTPS
- [ ] Know GET request purpose
- [ ] Understand reproducibility importance

**Libraries:**

- [ ] Know which libraries are built-in vs pip install
- [ ] Understand urllib vs requests differences
- [ ] Know when to use each library

**BeautifulSoup:**

- [ ] Memorize key methods: .get_text(), .find_all(), .prettify()
- [ ] Know how to extract links (href attribute)
- [ ] Understand tag finding

**JSON:**

- [ ] Know json.load() for files, json.loads() for strings
- [ ] Understand JSON loads as dictionary
- [ ] Can access nested JSON data

**APIs:**

- [ ] Understand query string format (? and &)
- [ ] Know how to pass parameters
- [ ] Can use r.json() to parse responses

**Twitter:**

- [ ] Memorize 4 authentication credentials
- [ ] Know OAuth is the protocol
- [ ] Remember track parameter for filtering

**Code Practices:**

- [ ] Always use context managers (with statement)
- [ ] Know .text vs .json()
- [ ] Remember to close resources

### During the Exam ✓

**General Tips:**

- [ ] Read each question carefully
- [ ] Eliminate obviously wrong answers
- [ ] Watch for built-in vs install questions

**Code Questions:**

- [ ] Check for missing imports
- [ ] Look for unclosed files
- [ ] Verify correct method calls (.text vs .json())
- [ ] Check query string format (? first, then &)

**Common Traps:**

- [ ] json.load vs json.loads
- [ ] .text vs .json() for API responses
- [ ] urllib (built-in) vs requests (install)
- [ ] Query string starts with ? not &
- [ ] 4 credentials for Twitter (not 2 or 3)

**Quick Checks:**

- [ ] urllib = complex, requests = simple
- [ ] BeautifulSoup: .get_text() removes tags
- [ ] JSON → Python dict
- [ ] OAuth = Twitter authentication
- [ ] Reproducibility = main reason to automate

---

## Additional Resources

**Official Documentation:**

- requests: https://requests.readthedocs.io/
- BeautifulSoup: https://www.crummy.com/software/BeautifulSoup/
- Python json: https://docs.python.org/3/library/json.html
- Tweepy: https://docs.tweepy.org/

**Practice Sites:**

- JSONPlaceholder: https://jsonplaceholder.typicode.com/ (fake API)
- httpbin.org: Test HTTP requests
- Random User API: https://randomuser.me/api/

**Key Points to Remember:**

1. **Reproducibility** - Automate everything
2. **requests > urllib** - Simpler and more powerful
3. **BeautifulSoup** - Makes HTML parsing easy
4. **JSON = dict** - Direct Python mapping
5. **OAuth** - Industry standard authentication
6. **Context managers** - Always use `with`
7. **Query strings** - Start with `?`, separate with `&`

---

## Congratulations! 🎉

You've mastered:

- ✅ Web data downloading (urllib, requests)
- ✅ HTML parsing and web scraping (BeautifulSoup)
- ✅ API interactions and query parameters
- ✅ JSON data handling
- ✅ Twitter API with OAuth (Tweepy)
- ✅ Reproducible data workflows

**You're ready to import data from any web source!** 🌐🐍

---

#python #web-scraping #api #json #twitter #http #beautifulsoup #requests #datacamp #certification