# Entity Extraction Engine for Narrative Data
## User Guide

### 1. SETUP AND INSTALLATION

1. Retrieve the code by either:
- visiting the code repository and cloning the repository to your desired storage location
- download the files to your desired storage location

2. Install the spaCy environment: NOTE: You must install a new virtual environment to use the Entity Extraction Engine.

3. First, open a terminal window in the project directory.

4. Then choose one of the install methods below.
*NOTE: If you do not have permission to create or change environments, you can provide your system administrator with the requirements.txt in the root directory.*

a. Installing with Pip

    ```
    pip install virtualenv
    virtualenv venv
    source venv/bin/activate
    pip install -U pip setuptools wheel
    pip install -U spacy
    python -m spacy download en_core_web_trf
    python -m spacy download en_core_web_sm
    pip install -r requirements.txt
    ```


b. Installing with Conda or other Package Managers To use another package manager like conda, visit spacy.io/usage to use a tool that generates the code for your spaCy environment, then:

1. First, create a virtual environment in your project directory using the method for your package manager. Activate the environment in the project directory.
        e.g. conda create -name venv or similar
2. Choose your operating system/platform/package manager according to your system configuration.
3. Choose CPU or GPU. GPU will improve performance if you have it available.
   - Alternatively, see the comments in spacy_nlp.py to learn about CPU optimization options.
   - Choose virtual env as your configuration, and select pipeline for efficiency.
   - In a terminal, paste the output from the tool and execute.
   - See addtl_requirements.txt for additional dependencies to be installed, and install via conda, pip, or another available package manager.

4. Install related dependencies: A file named requirements.txt has been provided to install additional dependencies such as pandas and jupyter.
*NOTE: Depending on your system, you may also need to install an kernel spec to use Jupyter Notebooks. Check with your system administrator. See this document for installing an kernelspec: Ipykernel Documentation*

5. Retrieve Data: Text data should be pulled down from the database, using a CSV for the scripts.


## 2. FILE STRUCTURE Narrative Extraction

  ```
  ├── nlp_model                                       # spaCy model and guided notebooks
  │   ├── nbm1_pattern_config.ipynb                     # notebook to configure spaCy patterns
  │   ├── nbm2_review_extracts.ipynb                    # notebook to check model results           
  ├── notebooks                                       # documentation and guided noteboooks
  │   ├── nb1_handle_edge_cases.ipynb                   # run this notebook first to prep data/deal with edge cases 
  │   ├── nb2_sanity_check.ipynb                        # review data before auto-extraction
  │   └──  nb3_human_review_diverted.ipynb              # review edge cases
  ├── narrative_extraction_test.ipynb                 # test notebook
  ├── edge_cases.py                                   # script for edge cases
  ├── narrative_extraction.py                         # main script
  ├── spacy_nlp.py                                    # script for extraction engine
  └── transform.py                                    # script for data prep
  ```

## 3. Usage Guide
We recommend using the guided notebook approach while you get used to using the auto-extractor. This will help you to learn the most efficient ways to use the auto-extractor.

1. Notebook Workflow (notebooks/)

To execute the narrative extraction with the guided notebooks, follow the steps below.

BEFORE YOU START: Make a copy of each notebook, and add the date as a prefix. When you are finished, you can move the notebooks to the session directory (see item 4 below). Retain the original copy as a template.

- Start with notebooks/nb1_handle_edge_cases.ipynb to prepare the date for the auto-extraction engine. Follow the prompts in the notebook to remove duplicates, handle missing values, and detect edge cases.

- Use the notebook notebooks/nb2_sanity_check.ipynb to review the data that you will pass through the auto-extractor.

- Narratives that will "clog" the extraction engine are diverted away from the pipeline so they can be reviewed, using notebooks/nb3_review_diverted.ipynb.

- See inline comments and docstrings for detailed explanations of these processes.

- The scripts will automatically create a date/timestamped session directory for all of the output files in notebooks/DATE_TIME.

Example: If you ran the scripts at 10:00 AM on July 15, 2022 --
  ```
  ├── notebooks
  │   ├── 20220715_1000/
  │   │   ├── accepted/ 
  │   │   └── diverted/ 
  │   ├── nb1_handle_edge_cases.ipynb 
  │   ├── nb2_sanity_check.ipynb
  │   └── nb3_human_review_diverted.ipynb
  ```

## Script Workflow

*NOTE: When you have a large amount of data (more than 100,000 records) to process, we recommend using the nb2_sanity_check.ipynb to further filter edge cases.*

To execute the scripts without using the guided notebooks:

1. Proceed with caution. Using the guided notebooks the first few times will help you to get an understanding about what your data looks like, what kind of data will clog the auto-extractor.

2. To use the script workflow, complete the following steps:
- Open a new terminal window, and navigate to the folder containing narrative_extraction.py.
- Enter `python narrative_extraction.py`. This will trigger two scripts (transform.py, edge_cases.py) that transform the data to prepare for the extraction as well as detect edge cases, and send them to a separate directory for human review.
- Data that has been accepted for auto-extraction is passed through the auto-extractor. Rows requiring human review are sent to divert.
- Results appear in the session directory, which is automatically created in the notebooks folder.

Example: If you run the scripts at 10:00 AM on July 15, 2022 --

  ```
  ├── notebooks
  │   ├── 20220715_1000/
  │   │   ├── accepted/ 
  │   │   └── diverted/ 
  │   ├── nb1_handle_edge_cases.ipynb 
  │   ├── nb2_sanity_check.ipynb
  │   └── nb3_human_review_diverted.ipynb
  ```

3. Go to Notebook Workflow item 3 to conduct a human review on narratives that were diverted from the auto-extractor.

## 3. Building/Editing Models (nlp_model/)

Warning: Editing nlp_model/nbm1_pattern_config.ipynb and saving it to the name bootstrap_model/ will overwrite the existing model. Make a copy before you experiment, and use a name like test for your model until you are certain it is ready for production.

## 4. Resources
1. Read more about the spaCy model and patterns in this extraction engine in nlp_model/nbm1_pattern_config.ipynb. In this notebook, you can review, add, or delete the patterns that have been added to adjust the predictions of the statistical model.
2. Review the model's performance with nlp_model/nbm2_review_extracts.ipynb before running the extraction engine.

