# GPT Data Annotation Guide

This guide provides instructions for using GPT to perform generalized data annotation. The script processes data from an Excel spreadsheet and outputs one or more labels based on a specified prompt. Example categories include internal-external context, Value Chain Analysis activities, grammatical tense, Yes/No, and more.

## Table of Contents
1. [Introduction](#introduction)
2. [Setup](#setup)
   1. [Downloading Files](#downloading-files)
   2. [Preparing Data](#preparing-data)
   3. [OpenAI Key Generation](#openai-key-generation)
   4. [Prepare Script](#prepare-script)
   5. [Environment Setup](#environment-setup)
3. [Execution](#execution)
   1. [Running the Script](#running-the-script)
   2. [Expected Results](#expected-results)
4. [Conclusion and Notes](#conclusion-and-notes)
   1. [Runtime](#runtime)
   2. [Starting Over](#starting-over)
   3. [Stopping the Script](#stopping-the-script)
   4. [Backup Files](#backup-files)

## Introduction

This guide describes how to use GPT for generalized data annotation. The script processes data in an Excel spreadsheet and outputs one or more labels based on a specified prompt. Examples of categories include internal-external context, Value Chain Analysis activities, grammatical tense, Yes/No, and more.

## Setup

### Downloading Files

All files are available publicly on [GitHub](https://github.com/emilyhasson/gpt-annotator/tree/main). Select the "Code" button and then download as a .zip file. The contents of the folder should look like this:

## Setup

### Downloading Files

All files are available publicly on [GitHub](https://github.com/emilyhasson/gpt-annotator/tree/main). Select the "Code" button and then download as a .zip file. The contents of the folder should look like this:

```bash
gpt-annotator/
│
├── annotate.py
├── mentions.xlsx
├── guide.html
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

### Preparing Data

The data should be an Excel file with the following headings:

- `Filename`
- `Company`
- `Industry`
- `Date`
- `Sentence`
- `Context-Pre` (Optional)
- `Context-Post` (Optional)

Be sure to move your data file into the `gpt-annotator` folder.

### OpenAI Key Generation

To run this script, you will need an OpenAI API key. Visit the [OpenAI Website](https://openai.com), navigate to Products > API login, and log in or create an account. The site will guide you through creating a project and an API key. Set up billing on your account and copy the key somewhere safe for later use.

### Prepare Script

Before running the script to annotate your data, you will need to specify some details within the Python file based on your use case. 

Open `annotate.py` in a text editor, such as Visual Studio Code. Then, modify the following fields:

- `GPT_KEY`: Your OpenAI API key. Ex: `'pk-dj54xuw3n5549cwm'`
- `CONTEXT`: Set to `True` if your Excel data includes columns `Context-Pre` and `Context-Post`. Set to `False` if you only have a `Sentence` column to analyze.
- `CATEGORIES`: The list of categories you want GPT to sort your data into, stored as a Python list. Ex: `["Present", "Past", "Future"]`
- `MAX_CATEGORIES`: The maximum number of categories you want GPT to choose for a given sentence. Ex: `3`
- `PROMPT`: Your prompt for GPT. No need to specify the output format. Ex: `"Classify the grammatical tense of the following sentence. Choose between 'Past', 'Present', or 'Future'."`
- `DATA_SOURCE`: The name of your input Excel file. Ex: `"mentions.xlsx"`
- `BATCH_SIZE`: If the data is large, you may want to split the work by running the script multiple times on smaller chunks of the data. Ex: `1000`

### Environment Setup

Setting up the environment might differ based on your device. The easiest way is to use ChatGPT. Use the following prompt and follow the steps provided:

```plaintext
Walk me through the steps to set up my [Mac/Windows] device to run a Python script annotate.py in a virtual environment built from the requirements.txt file. Start with downloading and setting up Python.
```

ChatGPT can be very helpful for this type of task, so if you run into any problems or confusion, you can usually resolve it by asking follow-up questions.

## Execution

### Running the Script

Run the script in your terminal using the following command:

```bash
python annotate.py
```

If you get an error like "python not found," replace python with python3.

While the script runs, you should see a progress bar in your terminal. The process can take some time, especially for larger batches. If it takes very long (over 5 seconds per row), you might want to wait and try again later, or use a shorter prompt.

If you set a BATCH_SIZE smaller than the total length of your data, you will need to run the script multiple times. You don't need to change the script between runs. It's recommended to check your output data (results.xlsx) after each run (and close the file before running again).

### Expected Results

Your results will be saved to `results.xlsx`. The columns will be the same as before, with the addition of:

- `Output`: The raw GPT output with reasoning.
- `LogProbs`: The log probabilities.
- `Category_n`: The extracted categories.

## Conclusion and Notes

### Runtime

Querying GPT repeatedly can take considerable time. Running the script with a medium-length prompt and a batch size of 1,000 takes between 30 and 45 minutes. Running the entire S&P 500 database of about 12,000 sentences could take 6 to 9 hours altogether.

To avoid prolonged processing, split it into batches of 500 to 2,000 and run the script multiple times. This allows you to run it for 30 minutes or an hour at a time, rather than letting your computer run for several hours nonstop. The program is fairly lightweight and can run in the background.

### Starting Over

If you want to start over, delete `results.xlsx`, make your modifications, and run the script again.

### Stopping the Script

Stopping the script while it's running is not recommended, as it can cause issues with writing to Excel. Use a smaller `BATCH_SIZE` if you want to periodically check your output.

### Backup Files

After each batch, your `results.xlsx` will be copied to a backup file titled `BACKUP-OUTPUT-{timestamp}.xlsx`. These files can be useful if something catastrophic happens to the primary output file. To continue from that point, rename the most recent backup to `results.xlsx`. Otherwise, these backup files can safely be deleted.

---

If you run into any issues, feel free to reach out to the me.

--Emily
