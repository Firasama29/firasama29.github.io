---
title: Working with PDF files in Python
date: 2025-12-07
categories: [python]
tags: [python, pdf, modules]
---

# Working with PDF files in Python

Using Python for everyday uses is very useful as it's one of the easiest programming languages to work with, not only because of the clear, readable syntax but also of the wide range of available libraries that you can simply import and utilize in your projects.

### Merge multiple pdf files into one
Multiple PDF files can be combined into a single document for organizing reports, invoices or scanned documents. This can be done using Python's PyPDF2 library.

First, we'll need to install PyPDF2 library:
`pip install PyPDF2`
then we can import it in our Python script:

```python
  from PyPDF2 import PdfMerger
  import os
  
  folder = input("Enter your folder: ")
  output_file = "output.pdf"
  
  # validate input folder
  if not os.path.exists(folder):
    raise FileNotFoundError(f"input folder does not exist!")
    
  # validate output
  elif os.path.exists(output_file):
    raise FileExistsError(f"file `{output_file}` already exists!")
    
  # create PdfMerger object
  merger = PdfMerger()
  
  # loop through the input files ending with .pdf and append to the merger object
  for file in os.listdir(folder):
    if file.endswith(".pdf"):
      merger.append(os.path.join(folder, file))
      
  # save all the pdfs into a single pdf file
  merger.write(output_file)
  merger.close()
  print("pdf merged successfully!")
```

We've imported two modules:
- `PyPDF2` for handling PDFs.
- `os` allows us to list files from a specific folder.

`PyPDF2` provides functions to read, modify and create PDFs programmatically. Here the `append()` function is used to add the files to the merger object and keep them in memory until they're saved using `write()` function into the output file.
Calling `close()` simply releases resources and cleans up the merger object.

### Split pdf file into multiple single-page files
The following script utilizes `PyPDF2` to load a PDF file and save each page as an individual one-page document. Each page is extracted
from the `PdfReader` object, added to a new `PdfWriter` instance, and then saved as its own PDF file.

```python
  from PyPDF2 import PdfReader, PdfWriter
  
  # read the input PDF
  reader = PdfReader(input("Enter the path where the file is: "))
  
  # get total number of pages
  total_pages = len(reader.pages)
  
  # loop through each page and add save as separate PDF
  for i in range(total_pages):
    writer = PdfWriter()
    writer.add_page(reader.pages[i])
    
    output_name = f"page_{i + 1}.pdf"
    
    with open(output_name, "wb") as output_file:
      writer.write(output_file)
  
  print(f"Files split into {total_pages} pages") 
```

## Convert pdf to docs and vice versa
For conversion from PDF to DOCX, `pdf2docx` provides `Converter.convert()` function which simply performs the conversion.

It's a good practice to raise exceptions to flag invalid cases such as file not found or output already exists.

```python
  from pdf2docx import Converter
  import os
  
  pdf_file = input("Enter your input PDF file: ")
  docx_file = input("Enter your output docx file: ")
  
  # validate input PDF
  if not os.path.exists(pdf_file):
    raise FileNotFoundError(f"file `{pdf_file}` does not exist!")
    
  # validate output path
  elif os.path.exists(docx_file):
    raise FileExistsError(f"file `{docx_file}` already exists!")
  else:
    cv = Converter(pdf_file)
    cv.convert(docx_file)
    cv.close()
    print(f"{pdf_file} converted to {docx_file} successfully")
```

## Convert image to pdf
Converting images into PDF format is a common requirement for documents or reports. Python's `Pillow` or `PIL` (Python Imaging Library) library offers
a simple and reliable way to handle this task with a few lines of code. It loads the image, converts it into the appropriate color mode then exports it as a PDF.

```python
  from PIL import Image
  import os
  
  input_image = input("Enter the image file path: ")
  output_pdf = input("Enter the pdf file name: ")
  
  # validate input
  if not os.path.exists(input_image):
       raise FileNotFoundError(f"{input_image} does not exist!")
       
  # validate output
  elif os.path.exists(output_pdf):
       raise FileExistsError(f"{output_pdf} already exists!")
  
  # load and process image
  image = Image.open(input_image)
  image = image.convert("RGB")
  
  # save as pdf
  image.save(output_pdf)
  
  print("Image converted to PDF successfully")

```

## Convert multiple images to single pdf
Here's a similar use case for `PIL` library: combining multiple images into a single PDF document. The script below loads all images from a specified folder, converts them into appropriate format then merges them into a single multi-page PDF.

```python
  from PIL import Image
  import os
  
  folder = input("Enter the folder containing images: ")
  output_pdf = input("Enter the output PDF name: ")
  
  images = []
  
  for file in os.listdir(folder):
    if file.lower().endswith((".jpeg", ".jpg", ".png")):
      image = Image.open(os.path.join(folder, file)).convert("RGB")
      images.append(image)
  
  if images:
    # first page is required by PIL to start the PDF, so it acts as the base image
    first_image = images[0]
    
    # the remaining images
    others = images[1:]
    
    # create the PDF
    first_image.save(output_pdf, save_all=True, append_images=others)
    print("Images merged into one PDF successfully")
  else:
    print("No valid images found")
```

The scripts above showcase a glimpse of Python's versatility and power in automating file and document management tasks.
