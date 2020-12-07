# Egen_data_engineering


#### This Project was developed to complete an End-to-End Data Engineering using Docker, Apache Airflow, Python and PostGres Database.
1. First scan through COVID Data Sources from a JSON file which consists of other data sources.
2. Extract the URL which can help to download the Data.
3. Download the Data from the URLS and save it in a PostGres Database for future Analysis.
4. Schedule this Job to run everyday using Apache Airflow.


## Scan through COVID Data Sources in a JSON File.
#### 1. I used Jupyter Notebook to develop a Python Script to extract URL's which consists of Data Sources with COVID-19 Datasets.
#### 2. Below is an example of the JSON Data which explains about the various Data Sources present in the Online API's.

 {'@type': 'dcat:Dataset',
  'accessLevel': 'public',
  'bureauCode': ['009:00'],
  'contactPoint': {'fn': 'Medicaid',
   'hasEmail': 'mailto:Medicaid.gov@cms.hhs.gov'},
  'description': '<p>The CMS National Correct Coding Initiative (NCCI) promotes national correct coding methodologies and reduces improper coding which may result in inappropriate payments of Medicare Part B claims and Medicaid claims.</p>\n<p>NCCI procedure-to-procedure (PTP) edits define pairs of Healthcare Common Procedure Coding System (HCPCS)/Current Procedural Terminology (CPT) codes that should not be reported together for a variety of reasons. The purpose of the PTP edits is to prevent improper payments when incorrect code combinations are reported.</p>\n<p>Practitioner services also refers to ambulatory surgical centers.<br />\nDME refers to provider claims for durable medical equipment.</p>\n<p>The CMS National Correct Coding Initiative (NCCI) promotes national correct coding methodologies and reduces improper coding which may result in inappropriate payments of Medicare Part B claims and Medicaid claims. NCCI procedure-to-procedure (PTP) edits define pairs of Healthcare Common Procedure Coding System (HCPCS)/Current Procedural Terminology (CPT) codes that should not be reported together for a variety of reasons. The purpose of the PTP edits is to prevent improper payments when incorrect code combinations are reported. The edits in this dataset are active for the dates indicated within. This file should NOT be used by state Medicaid programs as their edit file. Current Procedural Terminology (CPT) codes, descriptions and other data only are copyright 2017 American Medical Association. All rights reserved. CPT® is a registered trademark of the American Medical Association. Applicable FARS\\DFARS Restrictions Apply to Government Use. Fee schedules, relative value units, conversion factors and/or related components are not assigned by the AMA, are not part of CPT, and the AMA is not recommending their use. The AMA does not directly or indirectly practice medicine or dispense medical services. The AMA assumes no liability for the data contained or not contained herein.</p>\n<p>For more information, visit <a href="https://www.medicaid.gov/medicaid/program-integrity/ncci/index.html">https://www.medicaid.gov/medicaid/program-integrity/ncci/index.html</a>.</p>\n',
  'distribution': [{'@type': 'dcat:Distribution',
    'downloadURL': 'https://data.medicaid.gov/api/views/34v9-f8up/rows.csv?accessType=DOWNLOAD',
    'format': 'csv',
    'mediaType': 'application/unknown',
    'title': 'csv'},
   {'@type': 'dcat:Distribution',
    'downloadURL': 'https://data.medicaid.gov/api/views/34v9-f8up/rows.rdf?accessType=DOWNLOAD',
    'format': 'rdf',
    'mediaType': 'application/unknown',
    'title': 'rdf'},
   {'@type': 'dcat:Distribution',
    'downloadURL': 'https://data.medicaid.gov/api/views/34v9-f8up/rows.json?accessType=DOWNLOAD',
    'format': 'json',
    'mediaType': 'application/unknown',
    'title': 'json'},
   {'@type': 'dcat:Distribution',
    'downloadURL': 'https://data.medicaid.gov/api/views/34v9-f8up/rows.xml?accessType=DOWNLOAD',
    'format': 'xsl',
    'mediaType': 'application/unknown',
    'title': 'xsl'}],
  'identifier': 'https://data.medicaid.gov/api/views/34v9-f8up',
  'issued': '2018-08-31',
  'keyword': ['cpt codes', 'hcpcs', 'ncci edits'],
  'landingPage': 'https://data.medicaid.gov/d/34v9-f8up',
  'modified': '2020-12-04',
  'programCode': ['009:076'],
  'publisher': {'@type': 'org:Organization',
   'name': 'Centers for Medicare & Medicaid Services'},
  'theme': ['Uncategorized'],
  'title': 'NCCI Procedure to Procedure Edits (PTP)'}}
  
  #### As the dataset is present in the above URL's given, we can iterate through this data and pick Keywords which consists of COVID-19, CORONAVIRUS and Severe Acute Respiratory disease as their respective keywords.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/1.PNG)
  
  #### After this, I extracted the URLS for every data source which consists these above Keywords.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/2.PNG)
  
  #### Now, after writing a function to download the Data in my local machine, I saved the Data in this manner.
  
   ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/3.PNG)
   
  #### Since the basic funtioning of my Data Extraction was ready, I deployed the Data in my PostGres Database using this function.
  
   ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/4.PNG)  
 
  #### The Data was successfully transferred in PostGres Database.
  
   ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/postgres_database_cdc_data.PNG)
   
  ## Scheduling this Job using Apache Airflow
  #### Airflow is a state-of-the-art Data Engineering Software developed by AirBnB, which helps to reiterate the Scripts that are developed to load the Data from a particular Database into the Database of your choice (Local Machine, Cloud Sources like AZURE, GCP and AWS).
  #### There are various parameters that we have to keep in mind while wiritng a Python Script for runnning as an ETL job in Airflow.
  ##### 1. Importing the Necessary Libraries.
  #### We can import python operator in order to communicate with Airflow that the function/script that we want to run is a Python File.
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/5.PNG)
  
  #### We should now give some default arguments so that we can schedule the number of times we want to run this Job.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/6.PNG)
  
  
  ###### ** Please note that the dag_id in the DAG function should be similar to the name of your Python Script in which you are writing these functions.** ######
  
  #### Create a function to include your whole end-to-end code which is functioning as expected in your Jupyter Notebook.
    
  ###### ** You can check this function in data.py file uploaded with this repository.** ######
  
  #### Next step is to call the Python Operator and execute the function with the associated DAG (Data- Acyclic Graph)
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/7.PNG)
  
  ###### ** Several Operators can be included and also they can be organized in the manner we want to execute the multiple operators.** ###### 
  ###### ** For Example: run_etl >> dummy_etl >> dummy_etl2** ######
  
  #### Triggering the DAG Created and checking its Status and Logs.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/8.PNG)
  
  #### Checking the Data which is stored in the Virtual Container for loading in PostGres Database.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/9.PNG)
  
  ## Copying the Data into my Local Machine and Loading it in PostGres Database.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/10.PNG)
  
  #### Database with the Required Data.
  
  ![ScreenShot](https://github.com/uttasarga9067/Egen_data_engineering/blob/main/postgres_database_cdc_data.PNG)
  
  
  
  ## Thank you for this Opportunity and I wish you liked my explanation
  
  
  
