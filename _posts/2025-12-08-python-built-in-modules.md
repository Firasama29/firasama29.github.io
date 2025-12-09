---
title: Built-in Modules in Python
date: 2025-12-08
categories: [python]
tags: [python, modules]
---

# Built-in Modules in Python

A module in python can refer to a file containing reusable code. A library is a collection of modules. It can either be imported from an external provider or pre-packaged with Python installation. And like in many other programming languages, a module simply contains a list of files/classes containing utility functions that you can use in your application.

Creating a module is as simple as creating a function in a python file and saving it:
```python
# file name: my_module.py

def display_message():
  print("Hello world!")
```
To use this file or _module_ we can import it:

```python
# file name: example.py
import my_module

my_module.display_message()
```
Unlike external modules, which need to be installed before importing, built-in modules already come pre-installed and ready to use.
They come with a wide range of functionalities from file operations, web services to system-related tasks.

The point of pre-packaging libraries and having them ready-to-use is to remove the hurdle of going through the tedious task of installing every required module, so developers can focus on development rather than setup.

The advantage of utilizing pre-installed libraries is not limited to the above:
- Reliability is a key factor, as they go through rigorous testing before being available for use by developers.
- Standardization, since they are part of Python's Standard Library, therefore they provide a standard way to perform tasks.
- They come with Python's official comprehensive documentation.
- They are regularly maintained by the core Python community, so updates, improvements and bug fixes are to be expected.

Here are some examples of commonly used built-in Python modules:
- ### os module
  offers OS-level functionality to interact with the operating system like reading directory, launching applications, etc.

```python
  import os
  
  # Get current working directory
  print("Current directory:", os.getcwd())
  
  # List files and folders in a directory
  print("Contents:", os.listdir("."))
  
  # Create a new folder
  os.mkdir("test_folder")
  
  # Remove a folder
  os.rmdir("test_folder")
  
  # Join paths safely (better than manual concatenation)
  full_path = os.path.join("my_folder", "file.txt")
  print("Path:", full_path)
  
```

- ### sys module
  provides variables and functions to interact with Python interpreter. Core features include inspecting Python version and retrieving environment details.
```python
  import sys
  
  print(f"Python version: {sys.version}")
  print(f"Python arguments: {sys.argv}")
```

- ### datetime module
  used for formatting dates, creating timestamps, parsing strings into dates and other date and time-related functionalities. 
```python
  from datetime import datetime, date
  
  now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
  print(f"Today's date & time: {now}")
  
  print(f"Today's date: {date.today()}")
  
  d = date(2025, 12, 08)
  print(d)
```

- ### math module
  provides access to common mathematical functions to perform various operations.
```python
  import math
  
  print(f"Square root of 25 = {math.sqrt(25)}")
  
  print(f"Pi: {math.pi}")
  
  print(f"5 to the power of 3 = {math.pow(5, 3)}")
```

- ### calendar module
  provides calendar-related functions such as generating calendars, iterating through months, days and checking leap years.
```python
  import calendar
  
  # print a calendar
  print(calendar.month(2025, 12))
  
  # output:
      December 2025
  Mo Tu We Th Fr Sa Su
   1  2  3  4  5  6  7
   8  9 10 11 12 13 14
  15 16 17 18 19 20 21
  22 23 24 25 26 27 28
  29 30 31
  
  # check for leap year
  print(f"is 2025 a leap year? {calendar.isleap(2025)}")  # False
  
  # iterate through weeks of a month
  for day in calendar.monthcalendar(2025, 12):
    print(day)
    
  # output
  [1, 2, 3, 4, 5, 6, 7]
  [8, 9, 10, 11, 12, 13, 14]
  [15, 16, 17, 18, 19, 20, 21]
  [22, 23, 24, 25, 26, 27, 28]
  [29, 30, 31, 0, 0, 0, 0]
```

The examples above only cover a fraction of Python's built-in modules. Python's official documentation offers a detailed look into available modules https://docs.python.org/3/py-modindex.html.
