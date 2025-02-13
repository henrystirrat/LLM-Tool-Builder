# LLM Tool Builder

## Overview
LLM Tool Builder is a desktop application that helps you create and run analysis tools for processing PDFs with Large Language Models (LLMs). The application allows you to define what data should be extracted, set system prompts, and test on multiple PDF documents in batch. The results are then displayed in the application.

## System Requirements
- Windows 10 or later
- 4GB RAM minimum
- 100MB free disk space

## API Requirements
- Microsoft Document Intelligence - Layout model deployment
- Azure OpenAI - Global deployment of any model with API version 2023-12-01-preview or newer
  - GPT-4o is recommended as of February 2025

>You can find a tutorial of how to create these deployments [here]()

## Directory Structure
All files that the user might need to access are stored in the `resources` folder The application uses the following directory structure:
```
resources/
├── tools/         # Stores your saved tool configurations
├── data/          # Contains application data and output files
├── pdfs/          # Place your PDF files here for analysis
├── csvs/          # Output directory for CSV results
├── config.txt     # Specify your API information here
└── ...            # Other files used by the application are stored here
                     They are not relevant to the user
```
## Installation and Setup
1. Extract all files from the zip archive to your desired location
2. Paste you API information into `config.txt`
```
Document Intelligence API key:
Document Intelligence API endpoint:
OpenAI API key:
OpenAI API endpoint:
```
3. Run `LLM-Tool-Builder.exe` to start the application

## Fields

There are several fields that can be controlled to change how data is extracted.

#### System Prompt

The system prompt provides instructions to the large language model. Here you should describe what kind of materials you are studying and tell the LLM to extract information. Prompt engineering is more of an art than a science, so try multiple prompts to see what works best. That said, here are some tips that tend to give better results:
  - Start with a role assignment e.g. "You are a materials scientist who is an expert on ..."
  - Explicitly mention all metrics you want to be extracted here. This ensures the LLM knows what to look for.
  - Explicitly tell the LLM to report data exactly as it is reported and not to make up information. This way the LLM should not guess or hallucinate any values.

If you are unsure how to start, use the following template:

> You are a materials scientist who is an expert on ... You are analyzing scientific papers. For each ... material studied in the paper, extract the following properties: ... Do not make up any information. Do not report any information that is not explicitly mentioned in the paper.

#### Metrics

This is where you define what data is extracted. You can think of each metric as a column in the output dataset. If a metric is not reported for a specific material, then that entry will simply be left blank. The first metric is required - data will not be extracted if it is not found. Therefore it should typically be something that identifies the material, like material name. Beyond this order does not matter.

> ***TIP:*** It is often desired to know the units that a value was reported in. To do this, add a second metric to capture the units that the original metric was reported in. Alternatively, you can include the desired units in the description and instruct the model to only include the value if the units match. Currently, asking the LLM to convert the units is unreliable.

There are three parameters that need to be defined for each metric:

###### Name
Should be something like the name of the property/information being extracted. This is how the column in the output dataset is labelled.

###### Description
This is where what information should be included for the metric. In simple cases, the description can be: `The value of [Name] reported for the material.` Add more context as needed.

###### Type
There are three options for the type of data:
  - Numeric
  - Text
  - Boolean (True or False)

It is important to choose the correct option or the data will not be extracted.

#### Folder Path of Papers to Analyze

Here you can specify the folder where the pdfs you want to analyze are. The folder path must be relative to the `resources` folder. It is recommended to use the default folder `resources/pdfs` unless you want to switch between testing multiple sets of papers.

#### File Name
Choose the name that will be used to save the information you have specified. It will be saved to `resources/tools` as `FileName.json`
## Usage

### Creating a New Tool
1. Enter your system prompt in the "System Prompt" text box
2. Add metrics by clicking the "Add Metric" button
3. For each metric, specify:
   - Name
   - Description
   - Type (Numeric, Boolean, or Text)
4. Set the folder path where your PDFs are located
5. Enter a file name for your tool configuration
6. Click "Save" to save your tool configuration

### Loading an Existing Tool
1. Click the "Load" button
2. Select your previously saved .json configuration file
3. The form will populate with your saved settings

### Running Analysis
1. Ensure your PDFs are in the specified folder path
2. Click the "Run" button to start the analysis
3. Wait for the process to complete
4. Results will appear in a table below the form
5. Results will also be saved to the csvs folder

## Costs

Since we are using paid APIs, there is a cost accrued every time the run button is pressed. The cost breaks down as follows:
#### Document Intelligence
$0.01 per page - This cost occurs the first time analysis is run on each PDF. The result from this is saved to be used again

#### Azure OpenAI
Price depends on model choice. Each model lists a price per 1M tokens for input and output. For this use case, price is dominated by input. 1M tokens equates to roughly 500 pages of research papers, so dividing the price of 1M tokens of input by 500 can be used to estimate the cost per page. This cost occurs every time the analysis is run.

>This tool was designed to test data extraction on a small set of papers. At a larger scale it is more cost effective to use Azure OpenAI's batch API. The information in the tool files can be processed to prepare a batch job. You can find a tutorial on this [here]()

## Troubleshooting
- Make sure API information is correctly specified
- For blank results, ensure system prompt and metrics are properly defined
- Please report any unexpected errors



For support or bug reports, please contact hstirrat@uw.edu
