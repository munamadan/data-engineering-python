
## 📚 What You Already Know

From previous courses:

- ✅ Import flat files (.txt, .csv)
- ✅ Import pickled files, Excel spreadsheets
- ✅ Import from relational databases
- ✅ All locally stored data

**New challenge:** What if data is online?

---

## 🌐 Why Automate Web Imports?

### Manual Method

- Go to URL
- Click download
- Save file

### Problems:

❌ **Not reproducible** - Manual steps can't be automated ❌ **Not scalable** - Can't handle multiple files efficiently

### Solution: Python Automation! ✅

---

## 🎯 What You'll Learn

1. Import and locally save datasets from the web
2. Load datasets into pandas DataFrames
3. Make HTTP requests (GET requests)
4. Scrape web data (HTML)
5. Parse HTML into useful data (BeautifulSoup)
6. Use urllib and requests packages

---

## 📦 The urllib Package

### Purpose

**urllib** provides an interface for fetching data across the web

### Key Function: urlopen()

- Similar to `open()` for local files
- Accepts **URLs** instead of file names

---

## 💾 Downloading Files from the Web

### Using urlretrieve()

**Function:** Downloads file from URL and saves it locally

```python
from urllib.request import urlretrieve

url = 'http://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv'

# Download and save file
urlretrieve(url, 'winequality-white.csv')

# Returns: ('winequality-white.csv', <http.client.HTTPMessage>)
```

**Parameters:**

- First: URL (source)
- Second: Local filename (destination)

**Returns:** Tuple with filename and HTTP message

---

## 🔗 Understanding URLs

### URL Definition

**URL** = Uniform/Universal Resource Locator

- References to web resources
- Specifies web addresses uniquely

### URL Components

**Example:** `http://datacamp.com`

|Component|Example|Description|
|---|---|---|
|Protocol identifier|`http:`|Communication protocol|
|Resource name|`datacamp.com`|Web address/domain|

---

## 🌐 HTTP Protocol

### What is HTTP?

**HTTP** = HyperText Transfer Protocol

- Foundation of data communication for the web
- **HTTPS** = More secure version of HTTP

### HTTP Requests

**Going to a website = Sending an HTTP request**

**GET request** = Request to retrieve data from a server

- `urlretrieve()` performs a GET request
- Most common type of HTTP request

### Related Technologies

**HTML** = HyperText Markup Language

- Language used to structure web pages
- What you see in web browsers

---

## 📡 GET Requests Using urllib

### Method 1: urllib (Built-in)

```python
from urllib.request import urlopen, Request

url = "https://www.wikipedia.org/"

# Create request object
request = Request(url)

# Open connection and get response
response = urlopen(request)

# Read HTML content
html = response.read()

# Close connection
response.close()
```

**Steps:**

1. Create `Request` object with URL
2. Open URL with `urlopen()`
3. Read content with `.read()`
4. Close connection with `.close()`

---

## 🚀 GET Requests Using requests Package

### Why Use requests?

**requests** is one of the most downloaded Python packages!

**Used by:**

- Her Majesty's Government
- Amazon
- Google
- Twitter
- Sony
- Obama for America
- NPR, Twilio
- Federal U.S. Institutions

### Using requests Package

**Much simpler syntax:**

```python
import requests

url = "https://www.wikipedia.org/"

# Make GET request
r = requests.get(url)

# Get text content
text = r.text
```

**Advantages:**

- Cleaner, more intuitive syntax
- No need to manually close connections
- Better error handling
- More features built-in

---

## 📊 urllib vs requests Comparison

|Feature|urllib|requests|
|---|---|---|
|**Built-in**|Yes|No (requires install)|
|**Syntax**|More verbose|Simple and clean|
|**Connection**|Manual close needed|Auto-managed|
|**Popularity**|Standard library|Most downloaded|
|**Ease of use**|Moderate|Very easy|

**Recommendation:** Use `requests` for most web tasks

---

## 🕷️ Web Scraping with BeautifulSoup

### Understanding HTML

**HTML** contains mix of data types:

- **Structured data:** Pre-defined model or organized manner
- **Unstructured data:** No clear structure

**Challenge:** Extract useful information from HTML

---

## 🍲 BeautifulSoup Package

### Purpose

**BeautifulSoup** = Parse and extract structured data from HTML

**Tagline:** "Make tag soup beautiful and extract information"

### Why "BeautifulSoup"?

HTML can be messy ("tag soup") - BeautifulSoup cleans it up!

---

## 🔍 Using BeautifulSoup

### Basic Setup

```python
from bs4 import BeautifulSoup
import requests

url = 'https://www.crummy.com/software/BeautifulSoup/'

# Get HTML content
r = requests.get(url)
html_doc = r.text

# Create BeautifulSoup object
soup = BeautifulSoup(html_doc)
```

---

### Prettifying HTML

**`.prettify()`** = Format HTML for readability

```python
print(soup.prettify())
```

**Output:** Neatly indented HTML structure

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html>
  <head>
    <meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
    <title>
      Beautiful Soup: We called him Tortoise because he taught us.
    </title>
    ...
  </head>
  <body>
    ...
  </body>
</html>
```

---

## 🔎 Exploring BeautifulSoup Methods

### Getting Title

```python
print(soup.title)
# Output: <title>Beautiful Soup: We called him Tortoise...</title>
```

### Getting Text Content

```python
print(soup.get_text())
# Output: Beautiful Soup: We called him Tortoise because he taught us.
# You didn't write that awful page. You're just trying to
# get some data out of it...
```

**`.get_text()`** extracts all text, removing HTML tags

---

### Finding All Links

**`.find_all()`** = Find all instances of a tag

```python
# Find all anchor tags (links)
for link in soup.find_all('a'):
    print(link.get('href'))
```

**Output:**

```
bs4/download/
#Download
bs4/doc/
#HallOfFame
https://code.launchpad.net/beautifulsoup
https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup
...
```

**Explanation:**

- `'a'` = anchor tag (links in HTML)
- `.get('href')` = Get the URL from the link

---

## 🎯 Key Concepts Summary

### Web Import Methods

|Method|Use Case|Package|
|---|---|---|
|`urlretrieve()`|Download and save file|urllib|
|`urlopen()`|Read web content|urllib|
|`requests.get()`|GET request (preferred)|requests|

### Web Scraping Tools

|Tool|Purpose|
|---|---|
|**requests**|Fetch web content|
|**BeautifulSoup**|Parse and extract from HTML|

### BeautifulSoup Methods

|Method|Purpose|
|---|---|
|`.prettify()`|Format HTML nicely|
|`.title`|Get title tag|
|`.get_text()`|Extract all text|
|`.find_all('tag')`|Find all instances of tag|
|`.get('attribute')`|Get attribute value|

---

## 💡 Best Practices

### When to Use Each Tool

**urllib:**

- Built into Python (no install)
- Simple file downloads
- Basic web requests

**requests:**

- More complex HTTP operations
- Better error handling
- Preferred for most web scraping

**BeautifulSoup:**

- Parsing HTML/XML
- Extracting specific elements
- Cleaning messy HTML

---

## 🔧 Common Workflow

### Download and Load Data

```python
# 1. Download file
from urllib.request import urlretrieve
urlretrieve(url, 'data.csv')

# 2. Load into pandas
import pandas as pd
df = pd.read_csv('data.csv')
```

### Scrape Web Data

```python
# 1. Get HTML content
import requests
r = requests.get(url)
html = r.text

# 2. Parse with BeautifulSoup
from bs4 import BeautifulSoup
soup = BeautifulSoup(html)

# 3. Extract data
data = soup.find_all('tag')
```

---

## 📝 Quick Reference

### Download File

```python
from urllib.request import urlretrieve
urlretrieve(url, 'filename.csv')
```

### GET Request (requests)

```python
import requests
r = requests.get(url)
content = r.text
```

### Parse HTML

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_content)
soup.prettify()
soup.get_text()
soup.find_all('tag')
```

---

## 🎓 Key Takeaways

1. **Automate web data imports** for reproducibility and scalability
2. **urllib** is built-in but **requests** is more user-friendly
3. **HTTP GET requests** retrieve data from web servers
4. **BeautifulSoup** parses HTML and extracts structured data
5. **`.find_all()`** finds all instances of an HTML tag
6. **Workflow:** Fetch → Parse → Extract

---

#python #web-scraping #urllib #requests #beautifulsoup #http #html #datacamp
## Section A: Multiple Choice (15 Questions)

**Q1.** What does URL stand for?

- [ ] A) Universal Resource Link
- [ ] B) Uniform Resource Locator
- [ ] C) Universal Reference Locator
- [ ] D) Uniform Reference Link

**Q2.** What does HTTP stand for?

- [ ] A) HyperText Transfer Protocol
- [ ] B) HyperText Transmission Protocol
- [ ] C) HighText Transfer Protocol
- [ ] D) HyperText Transport Protocol

**Q3.** Which function downloads a file from a URL?

- [ ] A) urlopen()
- [ ] B) urlretrieve()
- [ ] C) download()
- [ ] D) get_file()

**Q4.** What package is one of the most downloaded Python packages?

- [ ] A) urllib
- [ ] B) pandas
- [ ] C) requests
- [ ] D) numpy

**Q5.** What does BeautifulSoup do?

- [ ] A) Downloads files
- [ ] B) Parses and extracts data from HTML
- [ ] C) Creates websites
- [ ] D) Compresses data

**Q6.** What type of HTTP request retrieves data?

- [ ] A) POST
- [ ] B) PUT
- [ ] C) GET
- [ ] D) DELETE

**Q7.** What does HTTPS provide over HTTP?

- [ ] A) Faster speed
- [ ] B) More security
- [ ] C) More data
- [ ] D) Better images

**Q8.** Which method extracts all text from HTML?

- [ ] A) .text()
- [ ] B) .get_text()
- [ ] C) .extract()
- [ ] D) .read_text()

**Q9.** What does `.find_all('a')` find?

- [ ] A) All paragraphs
- [ ] B) All links
- [ ] C) All headers
- [ ] D) All images

**Q10.** Which is simpler for GET requests?

- [ ] A) urllib
- [ ] B) requests
- [ ] C) Both equal
- [ ] D) Neither

**Q11.** What does `.prettify()` do?

- [ ] A) Downloads HTML
- [ ] B) Formats HTML for readability
- [ ] C) Deletes HTML
- [ ] D) Compresses HTML

**Q12.** What does HTML stand for?

- [ ] A) HyperText Markup Language
- [ ] B) HighText Markup Language
- [ ] C) HyperText Making Language
- [ ] D) HyperText Multi Language

**Q13.** What attribute contains the URL in an anchor tag?

- [ ] A) url
- [ ] B) link
- [ ] C) href
- [ ] D) src

**Q14.** Which package is built into Python?

- [ ] A) requests
- [ ] B) beautifulsoup
- [ ] C) urllib
- [ ] D) scrapy

**Q15.** Why automate web data imports?

- [ ] A) It's fun
- [ ] B) Reproducibility and scalability
- [ ] C) Required by law
- [ ] D) Faster internet

---

## Section B: True/False (10 Questions)

**Q16.** URL stands for Uniform Resource Locator. **T/F**

**Q17.** HTTP is the foundation of web data communication. **T/F**

**Q18.** urlretrieve() requires the requests package. **T/F**

**Q19.** requests.get() is simpler than urllib for GET requests. **T/F**

**Q20.** BeautifulSoup can parse HTML. **T/F**

**Q21.** Manual file downloads are reproducible and scalable. **T/F**

**Q22.** HTTPS is more secure than HTTP. **T/F**

**Q23.** .find_all() finds all instances of an HTML tag. **T/F**

**Q24.** urllib requires installation via pip. **T/F**

**Q25.** Structured data has a pre-defined data model. **T/F**

---

## Section C: Code Analysis (5 Questions)

**Q26.** What does this code do?

```python
from urllib.request import urlretrieve
urlretrieve('http://example.com/data.csv', 'data.csv')
```

- [ ] A) Reads a local file
- [ ] B) Downloads file from URL and saves it
- [ ] C) Uploads a file
- [ ] D) Deletes a file

**Q27.** What does this return?

```python
import requests
r = requests.get('http://example.com')
r.text
```

- [ ] A) Binary data
- [ ] B) HTML/text content
- [ ] C) URL
- [ ] D) Error

**Q28.** What does this code do?

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc)
soup.get_text()
```

- [ ] A) Gets HTML tags
- [ ] B) Extracts all text content
- [ ] C) Downloads webpage
- [ ] D) Creates HTML

**Q29.** What will this find?

```python
for link in soup.find_all('a'):
    print(link.get('href'))
```

- [ ] A) All URLs from links
- [ ] B) All images
- [ ] C) All paragraphs
- [ ] D) All titles

**Q30.** What's the purpose of this?

```python
print(soup.prettify())
```

- [ ] A) Compress HTML
- [ ] B) Format HTML readably
- [ ] C) Delete HTML
- [ ] D) Encrypt HTML

---

## Answer Key

### Section A

1. B 2. A 3. B 4. C 5. B 6. C 7. B 8. B 9. B 10. B
2. B 12. A 13. C 14. C 15. B

### Section B

16. T 17. T 18. F 19. T 20. T 21. F 22. T 23. T 24. F 25. T

### Section C

26. B 27. B 28. B 29. A 30. B

---

## Quick Reference

### Download File

```python
from urllib.request import urlretrieve
urlretrieve(url, 'filename.csv')
```

### GET Request

```python
# urllib method
from urllib.request import urlopen, Request
request = Request(url)
response = urlopen(request)
html = response.read()
response.close()

# requests method (preferred)
import requests
r = requests.get(url)
content = r.text
```

### Parse HTML

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_content)

# Methods
soup.prettify()        # Format HTML
soup.get_text()        # Extract text
soup.title             # Get title
soup.find_all('tag')   # Find all tags
```

### Key Concepts

- **URL:** Uniform Resource Locator
- **HTTP:** HyperText Transfer Protocol
- **GET:** Request type to retrieve data
- **HTML:** HyperText Markup Language
- **urlretrieve():** Download and save file
- **requests.get():** Make GET request
- **BeautifulSoup:** Parse HTML

### Package Comparison

|Package|Built-in|Use For|
|---|---|---|
|urllib|Yes|Basic downloads|
|requests|No|GET requests (easier)|
|BeautifulSoup|No|HTML parsing|

---

#python #quiz #web-scraping #urllib #requests #beautifulsoup #http