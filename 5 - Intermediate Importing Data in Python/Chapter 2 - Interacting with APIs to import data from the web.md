
## 📚 Chapter Topics

- What are APIs?
- JSON data format
- Loading and exploring JSONs
- Connecting to APIs
- Query strings and parameters

---

## 🔌 What is an API?

### Definition

**API** = Application Programming Interface

**Components:**

- Set of **protocols and routines**
- Bunch of code
- Allows **two software programs to communicate** with each other

### Think of APIs as:

- A waiter in a restaurant (takes your order to the kitchen)
- A translator between two systems
- A messenger between applications

### Why APIs Matter

**APIs are everywhere:**

- Weather apps getting data
- Social media sharing
- Payment processing
- Map services
- Data retrieval from web services

---

## 📄 What is JSON?

### Definition

**JSON** = JavaScript Object Notation

**Key Characteristics:**

- Format for **real-time server-to-browser communication**
- Created by **Douglas Crockford**
- **Human readable** text format
- Language-independent (despite "JavaScript" in name)

### Why JSON?

- Lightweight data format
- Easy for humans to read and write
- Easy for machines to parse and generate
- Standard for web APIs

---

## 🎬 JSON Structure Example

### Movie Data Example

```json
{
  'Title': 'Snakes on a Plane',
  'Year': '2006',
  'Rated': 'R',
  'Released': '18 Aug 2006',
  'Runtime': '105 min',
  'Genre': 'Action, Adventure, Crime',
  'Director': 'David R. Ellis',
  'Actors': 'Samuel L. Jackson, Julianna Margulies, Nathan Phillips',
  'Language': 'English',
  'Country': 'Germany, USA, Canada',
  'Awards': '3 wins & 7 nominations.',
  'imdbRating': '5.6',
  'imdbVotes': '114,668',
  'imdbID': 'tt0417148',
  'Type': 'movie'
}
```

**Structure:** Key-value pairs (like Python dictionaries!)

---

## 💻 Loading JSONs in Python

### Reading JSON Files

```python
import json

# Open and load JSON file
with open('snakes.json', 'r') as json_file:
    json_data = json.load(json_file)

# Check type
type(json_data)
# Output: dict
```

**Key Points:**

- Use `json.load()` to read JSON files
- JSON loads as a Python **dictionary**
- Use context manager (`with`) for proper file handling

---

## 🔍 Exploring JSON Data

### Iterating Through JSON

```python
# Loop through key-value pairs
for key, value in json_data.items():
    print(key + ':', value)
```

**Output:**

```
Title: Snakes on a Plane
Country: Germany, USA, Canada
Response: True
Language: English
Awards: 3 wins & 7 nominations.
Year: 2006
Actors: Samuel L. Jackson, Julianna Margulies
Runtime: 105 min
Genre: Action, Adventure, Crime
imdbID: tt0417148
Director: David R. Ellis
imdbRating: 5.6
Rated: R
Released: 18 Aug 2006
```

### Accessing Specific Values

```python
# Access like a dictionary
print(json_data['Title'])      # Snakes on a Plane
print(json_data['Year'])       # 2006
print(json_data['imdbRating']) # 5.6
```

---

## 🌐 Connecting to APIs

### What You'll Learn

1. What APIs are
2. Why APIs are important
3. **In exercises:**
    - Connecting to APIs
    - Pulling data from APIs
    - Parsing data from APIs

---

## 📡 Making API Requests in Python

### Using requests Package

```python
import requests

# API URL with query
url = 'http://www.omdbapi.com/?t=hackers'

# Make GET request to API
r = requests.get(url)

# Convert response to JSON (dictionary)
json_data = r.json()

# Display data
for key, value in json_data.items():
    print(key + ':', value)
```

**Process:**

1. Import `requests`
2. Define API URL
3. Make GET request
4. Convert response to JSON with `.json()`
5. Work with data as dictionary

---

## 🔗 Understanding API URLs

### URL Breakdown

**Example:** `http://www.omdbapi.com/?t=hackers`

|Component|Example|Meaning|
|---|---|---|
|**Protocol**|`http`|Making an HTTP request|
|**Domain**|`www.omdbapi.com`|Querying the OMDB API|
|**Query string**|`?t=hackers`|Parameters for the request|

---

### Query Strings Explained

**Format:** `?parameter=value`

**In the example:**

- `?` = Start of query string
- `t=` = Parameter name (title)
- `hackers` = Parameter value

**Multiple parameters:**

```
?t=hackers&y=1995&plot=full
```

- `&` separates multiple parameters
- `t=hackers` - title parameter
- `y=1995` - year parameter
- `plot=full` - plot parameter

---

## 🎬 OMDB API Example

### What is OMDB?

**OMDB** = Open Movie Database

- API for movie information
- Similar to IMDB
- Free to use (with API key)

### Common Parameters

|Parameter|Symbol|Example|
|---|---|---|
|Title|`t`|`?t=inception`|
|Year|`y`|`?y=2010`|
|IMDB ID|`i`|`?i=tt1375666`|
|Type|`type`|`?type=movie`|
|Plot|`plot`|`?plot=full`|

---

## 🔄 JSON vs Python Dictionary

### Similarities

|Feature|JSON|Python Dict|
|---|---|---|
|Structure|Key-value pairs|Key-value pairs|
|Syntax|`{"key": "value"}`|`{"key": "value"}`|
|Access|By key|By key|

### Key Difference

**JSON:**

- Text format (string)
- For data transmission
- Must use double quotes for strings

**Python Dictionary:**

- Python object
- For data manipulation in code
- Can use single or double quotes

---

## 💡 Working with API Responses

### Complete Workflow

```python
import requests
import json

# 1. Make API request
url = 'http://api.example.com/data'
response = requests.get(url)

# 2. Check if successful
if response.status_code == 200:
    # 3. Convert to JSON/dictionary
    data = response.json()
    
    # 4. Access specific data
    print(data['title'])
    print(data['year'])
    
    # 5. Or save to file
    with open('data.json', 'w') as f:
        json.dump(data, f)
```

---

## 🎯 Key Methods Summary

### json Module Methods

|Method|Purpose|Example|
|---|---|---|
|`json.load()`|Load JSON from file|`json.load(file)`|
|`json.loads()`|Load JSON from string|`json.loads(string)`|
|`json.dump()`|Write JSON to file|`json.dump(data, file)`|
|`json.dumps()`|Convert to JSON string|`json.dumps(data)`|

### requests Methods for JSON

|Method|Purpose|
|---|---|
|`requests.get(url)`|Make GET request|
|`response.json()`|Convert response to dict|
|`response.text`|Get response as text|
|`response.status_code`|Check if successful (200)|

---

## 🔍 APIs Are Everywhere

### Common API Use Cases

**Social Media:**

- Twitter API - Get tweets
- Facebook API - Access posts
- Instagram API - Retrieve photos

**Data Services:**

- Weather APIs - Get forecasts
- Financial APIs - Stock prices
- News APIs - Latest articles

**Maps & Location:**

- Google Maps API
- OpenStreetMap API

**Entertainment:**

- Spotify API - Music data
- OMDB API - Movie information
- YouTube API - Video data

---

## 📊 JSON vs Other Formats

|Format|Human Readable|Common Use|File Extension|
|---|---|---|---|
|**JSON**|Yes|APIs, web|.json|
|**XML**|Moderate|APIs, configs|.xml|
|**CSV**|Yes|Tables|.csv|
|**Binary**|No|Storage|Various|

**Why JSON wins for APIs:**

- Lightweight
- Easy to parse
- Language independent
- Natural fit for web technologies

---

## 🎯 Best Practices

### API Requests

1. **Check documentation** - Know available endpoints
2. **Use API keys** - Required by most APIs
3. **Handle errors** - Check status codes
4. **Respect rate limits** - Don't overload servers
5. **Cache responses** - Avoid redundant requests

### JSON Handling

1. **Validate structure** - Check for expected keys
2. **Handle missing data** - Use `.get()` with defaults
3. **Pretty print** - Use `json.dumps(data, indent=2)`
4. **Type checking** - Confirm data types

---

## 🔧 Common Patterns

### Load JSON from File

```python
import json
with open('data.json', 'r') as f:
    data = json.load(f)
```

### Get Data from API

```python
import requests
r = requests.get('http://api.example.com/endpoint')
data = r.json()
```

### Save JSON to File

```python
with open('output.json', 'w') as f:
    json.dump(data, f, indent=2)
```

### Access Nested JSON

```python
# JSON: {"user": {"name": "John", "age": 30}}
name = data['user']['name']
age = data['user']['age']
```

---

## 📝 Quick Reference

### Import Required Packages

```python
import json
import requests
```

### API Request Flow

```python
# 1. Make request
response = requests.get(url)

# 2. Get JSON data
data = response.json()

# 3. Access data
value = data['key']
```

### JSON File Operations

```python
# Read
with open('file.json', 'r') as f:
    data = json.load(f)

# Write
with open('file.json', 'w') as f:
    json.dump(data, f)
```

### Query String Format

```
Base URL + ? + parameter=value & parameter2=value2
http://api.com/endpoint?key1=value1&key2=value2
```

---

## 🎓 Key Takeaways

1. **API** = Interface for software to communicate
2. **JSON** = Lightweight, human-readable data format
3. **JSON loads as Python dictionary** in Python
4. **requests.get()** makes API calls
5. **response.json()** converts API response to dictionary
6. **Query strings** add parameters to URLs with `?` and `&`
7. **APIs are everywhere** - weather, social media, finance, etc.
8. **JSON is standard** for web APIs due to simplicity and readability

---

#python #api #json #requests #web-services #http #datacamp 


## Section A: Multiple Choice (15 Questions)

**Q1.** What does API stand for?

- [ ] A) Application Program Interface
- [ ] B) Application Programming Interface
- [ ] C) Applied Programming Interface
- [ ] D) Application Process Interface

**Q2.** What does JSON stand for?

- [ ] A) JavaScript Object Notation
- [ ] B) Java Script Object Notation
- [ ] C) JavaScript Online Notation
- [ ] D) Java Object Notation

**Q3.** Who created JSON?

- [ ] A) Brendan Eich
- [ ] B) Douglas Crockford
- [ ] C) Guido van Rossum
- [ ] D) Tim Berners-Lee

**Q4.** What Python type does JSON load as?

- [ ] A) list
- [ ] B) tuple
- [ ] C) dictionary
- [ ] D) string

**Q5.** Which function loads JSON from a file?

- [ ] A) json.read()
- [ ] B) json.load()
- [ ] C) json.open()
- [ ] D) json.get()

**Q6.** What method converts API response to JSON?

- [ ] A) .to_json()
- [ ] B) .get_json()
- [ ] C) .json()
- [ ] D) .parse()

**Q7.** What character starts a query string in a URL?

- [ ] A) &
- [ ] B) #
- [ ] C) ?
- [ ] D) /

**Q8.** What character separates multiple parameters in a URL?

- [ ] A) ?
- [ ] B) &
- [ ] C) ,
- [ ] D) ;

**Q9.** What does an API allow?

- [ ] A) File compression
- [ ] B) Two software programs to communicate
- [ ] C) Data encryption
- [ ] D) Image processing

**Q10.** What is JSON primarily used for?

- [ ] A) Database storage
- [ ] B) Server-to-browser communication
- [ ] C) File compression
- [ ] D) Image formats

**Q11.** Is JSON human readable?

- [ ] A) No
- [ ] B) Yes
- [ ] C) Only for programmers
- [ ] D) Only in browsers

**Q12.** What package is used to make API requests?

- [ ] A) urllib
- [ ] B) requests
- [ ] C) json
- [ ] D) api

**Q13.** In `?t=hackers`, what does `t` represent?

- [ ] A) Token
- [ ] B) Type
- [ ] C) Parameter name (title)
- [ ] D) Tag

**Q14.** What structure does JSON use?

- [ ] A) Arrays only
- [ ] B) Key-value pairs
- [ ] C) Lists only
- [ ] D) Tables

**Q15.** What HTTP method gets data from an API?

- [ ] A) POST
- [ ] B) PUT
- [ ] C) GET
- [ ] D) DELETE

---

## Section B: True/False (10 Questions)

**Q16.** API stands for Application Programming Interface. **T/F**

**Q17.** JSON is only used with JavaScript. **T/F**

**Q18.** JSON loads as a Python dictionary. **T/F**

**Q19.** Query strings start with `?` in URLs. **T/F**

**Q20.** APIs allow software programs to communicate. **T/F**

**Q21.** JSON was created by Douglas Crockford. **T/F**

**Q22.** `response.json()` converts API response to dictionary. **T/F**

**Q23.** Multiple URL parameters are separated by `&`. **T/F**

**Q24.** JSON is not human readable. **T/F**

**Q25.** APIs are only used for social media. **T/F**

---

## Section C: Code Analysis (5 Questions)

**Q26.** What does this code do?

```python
import json
with open('data.json', 'r') as f:
    data = json.load(f)
```

- [ ] A) Writes JSON to file
- [ ] B) Reads JSON from file
- [ ] C) Deletes JSON file
- [ ] D) Creates JSON file

**Q27.** What does this return?

```python
import requests
r = requests.get('http://api.example.com/data')
r.json()
```

- [ ] A) String
- [ ] B) Dictionary
- [ ] C) List
- [ ] D) Tuple

**Q28.** What does this loop do?

```python
for key, value in json_data.items():
    print(key, value)
```

- [ ] A) Prints JSON file path
- [ ] B) Prints all key-value pairs
- [ ] C) Creates new JSON
- [ ] D) Deletes keys

**Q29.** In this URL, what is the parameter value?

```
http://api.com/?title=inception
```

- [ ] A) api.com
- [ ] B) title
- [ ] C) inception
- [ ] D) http

**Q30.** What type is `json_data` after this?

```python
import json
with open('movie.json', 'r') as f:
    json_data = json.load(f)
type(json_data)
```

- [ ] A) str
- [ ] B) list
- [ ] C) dict
- [ ] D) json

---

## Answer Key

### Section A

1. B 2. A 3. B 4. C 5. B 6. C 7. C 8. B 9. B 10. B
2. B 12. B 13. C 14. B 15. C

### Section B

16. T 17. F 18. T 19. T 20. T 21. T 22. T 23. T 24. F 25. F

### Section C

26. B 27. B 28. B 29. C 30. C

---

## Quick Reference

### Key Concepts

- **API:** Application Programming Interface - allows software to communicate
- **JSON:** JavaScript Object Notation - human-readable data format
- **Query String:** Parameters in URL starting with `?`
- **GET Request:** HTTP method to retrieve data

### Load JSON from File

```python
import json
with open('file.json', 'r') as f:
    data = json.load(f)
```

### Get Data from API

```python
import requests
r = requests.get('http://api.example.com/endpoint')
data = r.json()  # Convert to dictionary
```

### Access JSON Data

```python
# JSON loads as dictionary
title = data['Title']
year = data['Year']

# Loop through
for key, value in data.items():
    print(key, value)
```

### URL Query Strings

```
Base URL + ? + parameters
http://api.com/endpoint?param1=value1&param2=value2
```

- `?` starts query string
- `&` separates parameters
- `param=value` format

### JSON Methods

```python
json.load(file)      # Read from file
json.loads(string)   # Parse from string
json.dump(data, file)  # Write to file
json.dumps(data)     # Convert to string
```

### requests Methods

```python
requests.get(url)     # Make GET request
response.json()       # Convert to dict
response.text         # Get as text
response.status_code  # Check success
```

### Common Status Codes

- **200:** Success
- **404:** Not Found
- **500:** Server Error

---

#python #quiz #api #json #requests #web-services