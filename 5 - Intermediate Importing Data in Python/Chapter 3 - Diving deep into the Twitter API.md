
## 📋 Overview

This chapter covers streaming data from Twitter API, filtering tweets, authentication protocols, and using the Tweepy Python package.

**Course:** Intermediate Importing Data in Python  
**Track:** Data Engineering with Python

---

## 🔑 Key Concepts

### Twitter API Basics

- **Purpose**: Access and stream Twitter data programmatically
- **Data Format**: Tweets are returned as [[JSON]] objects
- **Multiple APIs**: Twitter provides several different APIs for various use cases

### API Authentication & OAuth

- **Authentication Required**: All Twitter API requests require proper authentication
- **OAuth Protocol**: Industry-standard authorization framework used by Twitter
- **Four Credentials Needed**:
    1. `access_token` - User-specific token
    2. `access_token_secret` - Secret key for access token
    3. `consumer_key` - Application identifier (API key)
    4. `consumer_secret` - Secret key for consumer

### Tweepy Python Package

- **Purpose**: Python library for accessing Twitter API
- **Simplifies**: Authentication and data streaming processes
- **Main Functions**: Stream creation and tweet filtering

---

## 💻 Implementation

### Authentication Setup

```python
import tweepy, json

# Define credentials
access_token = "your_access_token"
access_token_secret = "your_access_token_secret"
consumer_key = "your_consumer_key"
consumer_secret = "your_consumer_secret"
```

### Streaming Tweets

```python
# Create Streaming object
stream = tweepy.Stream(consumer_key, consumer_secret,
                       access_token, access_token_secret)

# Filter Twitter Streams by keywords
stream.filter(track=['apples', 'oranges'])
```

---

## ⭐ Important Features

### Tweet Filtering

- **Keyword Filtering**: Use `track` parameter to filter tweets by specific keywords
- **Multiple Keywords**: Pass a list of keywords to filter multiple terms
- **Real-time Streaming**: Data is streamed in real-time as tweets are posted

### JSON Response Format

- Tweets returned as structured JSON objects
- Contains metadata: user info, timestamp, location, engagement metrics
- Enables easy parsing and data extraction in Python

---

## 📚 Course Context

### Topics Covered in Full Course:

1. Importing text files and flat files
2. Importing files in other formats
3. Writing SQL queries
4. Getting data from relational databases
5. Pulling data from the web
6. Pulling data from APIs (including Twitter API)

---

## 🎯 Exam Focus Points

> [!important] Key Takeaways
> 
> - Understand OAuth authentication requirements
> - Know the **four authentication credentials** needed
> - Remember **Tweepy** is the main Python package for Twitter API
> - Understand how to filter streams with keywords using `track` parameter
> - Know that Twitter returns data in **JSON format**
> - Be able to identify correct syntax for creating streams and filtering

---

## 🔗 Related Topics

- [[JSON Data Format]]
- [[OAuth Authentication]]
- [[API Integration]]
- [[Data Streaming]]
- [[Python Tweepy Library]]

---

**Last Updated:** {{date}}  
**Status:** #complete
## 📝 Multiple Choice Questions

### Q1: Twitter API Data Format

**What format does the Twitter API return tweet data in?**

- [ ] a) XML
- [ ] b) CSV
- [ ] c) JSON
- [ ] d) Plain text

> [!success] Answer **c) JSON** - Twitter API returns all tweet data as JSON objects

---

### Q2: Python Package for Twitter

**Which Python package is commonly used to access the Twitter API?**

- [ ] a) TwitterPy
- [ ] b) Tweepy
- [ ] c) PyTwitter
- [ ] d) APITwitter

> [!success] Answer **b) Tweepy** - The standard Python library for Twitter API access

---

### Q3: Authentication Credentials

**How many authentication credentials are required to access the Twitter API?**

- [ ] a) 2
- [ ] b) 3
- [x] c) 4
- [ ] d) 5

> [!success] Answer **c) 4** credentials needed:
> 
> - access_token
> - access_token_secret
> - consumer_key
> - consumer_secret

---

### Q4: Authentication Protocol

**What authentication protocol does Twitter API use?**

- [ ] a) Basic Auth
- [ ] b) API Key only
- [x] c) OAuth
- [ ] d) JWT

> [!success] Answer **c) OAuth** - Industry-standard authorization protocol

---

### Q5: Keyword Filtering

**Which parameter is used to filter Twitter streams by keywords?**

- [ ] a) filter
- [ ] b) keywords
- [x] c) track
- [ ] d) search

> [!success] Answer **c) track** - Used in `stream.filter(track=['keyword'])`

---

## 💾 Code Completion Questions

### Q6: Create Twitter Stream

**Complete the code to create a Twitter stream:**

```python
import tweepy, json

stream = tweepy.________(consumer_key, consumer_secret,
                         access_token, access_token_secret)
```

> [!success] Answer
> 
> ```python
> stream = tweepy.Stream(consumer_key, consumer_secret,
>                        access_token, access_token_secret)
> ```

---

### Q7: Filter by Keywords

**Complete the code to filter tweets by the keywords 'python' and 'data':**

```python
stream.filter(______=['python', 'data'])
```

> [!success] Answer
> 
> ```python
> stream.filter(track=['python', 'data'])
> ```

---

## ✅ True/False Questions

### Q8: Token Equivalence

**The access_token and consumer_key are the same thing in Twitter API authentication.**

- [ ] True
- [x] False

> [!success] Answer **False** - They serve different purposes:
> 
> - `consumer_key`: Identifies the application
> - `access_token`: User-specific authentication

---

### Q9: Real-time Streaming

**Tweepy can stream tweets in real-time as they are posted.**

- [x] True
- [ ] False

> [!success] Answer **True** - Tweepy enables real-time streaming of tweets

---

### Q10: Partial Authentication

**You only need consumer_key and consumer_secret to authenticate with Twitter API.**

- [ ] True
- [x] False

> [!success] Answer **False** - All **four credentials** are required for complete authentication

---

## 📖 Short Answer Questions

### Q11: Authentication Credentials

**What are the four authentication credentials needed for Twitter API access?**

> [!success] Answer
> 
> 1. `access_token`
> 2. `access_token_secret`
> 3. `consumer_key`
> 4. `consumer_secret`

---

### Q12: Track Parameter Purpose

**Explain the purpose of the `track` parameter in the `stream.filter()` method.**

> [!success] Answer The `track` parameter filters incoming tweets based on specific keywords. It accepts a list of keywords and only captures tweets containing those keywords, enabling targeted data collection from the Twitter stream.

---

### Q13: JSON Format Advantages

**Why is JSON a suitable format for Twitter API responses?**

> [!success] Answer JSON is:
> 
> - Structured and human-readable
> - Easily parseable in Python
> - Capable of representing complex nested data structures
> - Ideal for tweet metadata, user info, and engagement metrics

---

## 🎯 Scenario-Based Questions

### Q14: Real-time Tweet Collection

**You need to collect tweets about "machine learning" and "artificial intelligence" in real-time. Write the complete code to set up authentication and stream these tweets.**

> [!success] Answer
> 
> ```python
> import tweepy, json
> 
> # Authentication credentials
> access_token = "your_access_token"
> access_token_secret = "your_access_token_secret"
> consumer_key = "your_consumer_key"
> consumer_secret = "your_consumer_secret"
> 
> # Create streaming object
> stream = tweepy.Stream(consumer_key, consumer_secret,
>                        access_token, access_token_secret)
> 
> # Filter by keywords
> stream.filter(track=['machine learning', 'artificial intelligence'])
> ```

---

### Q15: Authentication Failure

**What would happen if you tried to access the Twitter API without proper authentication credentials?**

> [!success] Answer The API request would fail and return an authentication error. OAuth authentication is mandatory, and all four credentials must be valid and properly configured for successful API access.

---

## 📝 Fill in the Blanks

### Q16

Twitter uses the **________** protocol for API authentication.

> [!success] Answer **OAuth**

---

### Q17

The **________** Python library simplifies accessing the Twitter API.

> [!success] Answer **Tweepy**

---

### Q18

Twitter API returns tweets as **________** objects.

> [!success] Answer **JSON**

---

### Q19

To filter tweets by specific keywords, use the **________** parameter in the filter method.

> [!success] Answer **track**

---

### Q20

The two types of secrets needed are access_token_secret and **________**.

> [!success] Answer **consumer_secret**

---

## 📊 Question Summary

**Total Questions:** 20

- Multiple Choice: 5
- Code Completion: 2
- True/False: 3
- Short Answer: 3
- Scenario-Based: 2
- Fill in the Blanks: 5

---

**Related Notes:** [[Chapter 3 - Twitter API & Authentication]]  
**Last Updated:** {{date}}  
**Status:** #practice-ready