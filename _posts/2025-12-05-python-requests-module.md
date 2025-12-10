---
title: Python's Requests module
date: 2025-12-05
categories: [python]
tags: [python, requests, modules]
---

Requests library or module is one of python's most widely used libraries for making HTTP requests. It provides an intuitive API that supports all major HTTP methods like `GET`, `POST`, `PUT`, `DELETE`, without the need to manually handle low-level networking details.

### Syntax:
`requests.methodname(params)`

### Core features:
Below are some of the core capabilities of Requests:
- Simplifies HTTP requests with functions like `get()`, `post()`, `put()`, `delete()`, `patch()`
- Provides clean and simple management of headers, parameters and cookies
- Supports sessions, timeouts, error handling and authentication
- Built-in JSON parsing

### Example of using `requests`:
- **Simple get and posts requests:** Here's an example of a `get` request that returns data of a country and another example of `post` request that takes whatever payload you send and returns it back in the response.
```python
  import requests
  
  # GET request:
  get_response = requests.get("https://restcountries.com/v3.1/name/malaysia")
  country = get_response.json()[0]
  
  print(f"Country: {country["name"]["common"]}")
  print(f"Capital: {country["capital"][0]}")
  
  
  # POST request
  payload = {
    "name": "User",
    "email": "user@example.com"
  }
  
  post_response = requests.post("https://httpbin.org/post", json=payload)
  print(f"status code: {post_response.status_code}")
  print(f"response: {post_response.json()}")
  
```

- **Handling headers, query params and errors:**
```python
  headers = {
    "Accept": "application/vnd.github+json"
  }
  
  params = {
    "q": "python",
    "sort": "stars",
    "order": "desc"
  }
  
  try:
    response = requests.get("https://api.github.com/search/repositories", headers=headers, params=params)
    data = response.json()
  
    top_repo = data["items"][0]
    print("Name:", top_repo["full_name"])
    print("Stars:", top_repo["stargazers_count"])
    print("URL:", top_repo["html_url"])
    
  except requests.exceptions.Timeout:
    print("Request timed out. The server took too long to response.")
  except requests.exceptions.JSONDecoderError:
    print("Response returned but could not be decoded as JSON.")
  except Exception as e:
    print("An unexpected error occured: {e}")
```
