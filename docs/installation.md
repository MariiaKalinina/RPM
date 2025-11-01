# Installation and Setup Guide

This guide provides step-by-step instructions for installing and running the forward_isotropic_solver.py script in Google Colab.

## Prerequisites
- A Google account
- Web browser with internet access
- Input files (e.g., Input-1.txt, Input-2.txt, Input-3.txt)

## Step 1: Open Google Colab

1. Go to [colab.research.google.com](https://colab.research.google.com) in your web browser.
2. Sign in with your Google account if prompted.
3. If you have the provided **main.ipynb** file, upload it:
   - Click **File** → **Upload notebook**
   - Select your main.ipynb file
   - Alternatively, create a new notebook by clicking **New notebook**

## Step 2: Upload Your Files

You need to upload the following files to Google Colab:

### Required Files:
- The Python script: **forward_isotropic_solver.py**
- The input file(s): e.g., **Input-1.txt**, **Input-2.txt**, **Input-3.txt**

### Upload Instructions:

1. In the Colab notebook interface, look for the left sidebar and click the **Files** icon (folder symbol).
2. Click **Upload to session storage** button.
3. Select your files from your computer.
4. Wait for the upload to complete.

![File Upload Interface](/content/Upload_Files_(1).png)

**Note**: Files uploaded to Colab are temporary and will be removed when the runtime is disconnected. If you need to keep your files, save them to Google Drive or download them before ending your session.

## Step 3: Run the Script

### Method 1: Using Code Cells

1. Create a new code cell by clicking **+ Code** in the notebook toolbar.
2. Use the `!python` command to execute the script, passing the input file as an argument:

```python
!python forward_isotropic_solver.py Input-3.txt
