---
title: "Renaming Files in Bulk Using Python"
date: 2025-12-12
categories: [python]
tags: [python, scripts]
---

### Use case
You have a large number of files, and you're asked to rename them in some sort of order or naming scheme. The larger the number of files, the more time and effort required to achieve the expected outcome.  

This time and effort can be significantly reduced by automating the entire process using various tools such as online platforms, AI assistants or simply Python scripts.
Python is not only a friendly-syntax programming language, it's also exceptionally powerful and useful for automating everyday tasks.

Unlike online tools and AI, leveraging Python is particularly useful for us software developers to refine our coding and problem-solving skills, or — for developers fairly new to the language — to gain hands-on familiarity with it and simultaneously accomplish practical automation tasks.

In this article, we'll demonstrate a use case where we write a Python script to automate the task of renaming images with a prefix followed by a number in ascending order (e.g. "image_1", "image_2", "image_3", ...).

First, let's create a file and save it as `bulk_file_rename.py`. Inside it, we can write the following:
```python
  import os
  
  def rename_in_bulk(input_directory):
    try:
      os.chdir(input_directory)
    
      files = [f for f in os.listdir() if f.endswith((".png", ".jpg", ".jpeg"))]
      
      if not files:
          print("No images were found in this directory.")
          return
            
      n = 1
      for old_file in files:
        _, extension = os.path.splitext(old_file)
        new_file = f"image_{n}{extension}"
        n += 1
        os.rename(old_file, new_file)
        print(f"Renamed {old_file} -> {new_file}")
      print(f"File renaming completed. Files renamed: {n-1}")
    except FileNotFoundError:
      print("Directory not found")
    except Exception as e:
      print(f"An unexpected error occurred: {e}")
      
      
  rename_in_bulk(input("Enter directory path: "))
```

Executing this script in a directory that contains image files will perform the renaming action.

Here's a breakdown of the execution steps:

- `def rename_in_bulk(input_directory)`:

Defines a function, which accepts the target directory as an argument. The logic is wrapped inside a try/except block to catch potential errors.

- `os.chdir(input_directory)`:

Changes the current working folder to the target folder.

- `files = [f for f in os.listdir() if f.endswith((".png", ".jpg", ".jpeg"))]`:
  
Return a list of files in the folder but only files with these extensions `.png`, `.jpg`, `.jpeg`. 
 
So files contains only the images.

-   ```python      
      if not files:
        print("No images were found in this directory.")
        return
    ```

If no files were found in the folder, a message is displayed.

- `n = 1`:

Starts a counter from 1 which is to be incremented for each file. This will be used for naming images like `image_1.jpg`, `image_2.jpg`, etc.

- `for old_file in files:`

This loops through each image in the folder.

- `_, extension = os.path.splitext(old_file)`:

Splits the filename into:
  1. `_`: filename without extension, which will be ignored.

  2. `extension`: the extension part (e.g. ".jpg")` which we'll be using for the new filenames.

- `new_file = f"image_{n}{extension}"`:

Here is where we construct the new names of the images: `image_` + the counter `n` + `extension`.

- `n += 1`:

The counter is incremented every time we iterate through the images.

- `os.rename(old_file, new_file)`:

Function that actually renames each file by basically replacing the old file name with the new file name.

- `print(f"Renamed {old_file} → {new_file}")`:

Prints a message showing the old name → new name.

- `print(f"File renaming completed. Files renamed: {n-1}")`:

After the loop is done, this line print a message to show how many files were renamed.

- ```python
    except FileNotFoundError:
      print("Directory not found")
  ```
If the folder does not exist, we display a message in the except clause.

- ```python
    except Exception as e:
      print(f"An unexpected error occured: {e}")
  ```
This except clause catches any other error.

- `rename_in_bulk(input("Enter directory path: "))`

Lastly, we invoke our function and ask the user to enter the folder path containing the files.

To utilize the script, simply type this command on the terminal inside the directory where our python file is saved:
```shell
  python3 bulk_file_rename.py
```

Insert the target folder path, hit enter and the script will take care of the rest.<br>

Leveraging Python to automate repetitive tasks not only maximizes productivity but also maintains efficiency and accuracy and frees time for higher-value work.
