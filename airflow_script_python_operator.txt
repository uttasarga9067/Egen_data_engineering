from datetime import timedelta
from airflow.models import DAG
from airflow.utils.dates import days_ago
from airflow.operators.python_operator import PythonOperator
from airflow.operators.postgres_operator import PostgresOperator

default_args = {
    'owner': 'uttasarga',
    'start_date': days_ago(1),
    'email': ['uttasargasingh9067@gmail.com'],
    'email_on_faliure': False,
    'email_on_retry' : False,
    'retries':1,
    'retry_delay':timedelta(minutes=1)
}

dag = DAG(
    dag_id = 'data',
    default_args = default_args,
    description = 'Egen Data Dag',
    schedule_interval = timedelta(days=1)
)

def egen_data_etl():
    import numpy as np
    import pandas as pd
    import json
    import re
    import random
    import urllib.request, json 
    with urllib.request.urlopen("https://healthdata.gov/data.json") as url:
        data = json.loads(url.read().decode())

    # import json
    # file = open("C:/Users/uttas/Desktop/Resume/Egen Solutions/data.json")
    # data = json.load(file)
    # print(data)
        
    data_keywords= []
    for i in data['dataset']:
        keyword = i['keyword']
    # print(keyword)
        data_keywords.append(keyword)


    import itertools
    flat_list = list(itertools.chain(*data_keywords))
    df_keywords = pd.DataFrame(flat_list)
    covid_keys = df_keywords.loc[(df_keywords[0].str.contains('covid') == True) | (df_keywords[0].str.contains('coronav')  == True)].drop_duplicates()
    
    
    df = pd.DataFrame(columns = ['Keyword','downloadURL'])
    for i in range(len(data['dataset'])):
  # print(i)
        keyword = data['dataset'][i]['keyword']
        download_url = data['dataset'][i].get('distribution', 'Not Found')
        if(download_url !='Not Found'):
            if "downloadURL" in download_url[0]:
                download_url = download_url[0]['downloadURL']
            elif ("accessURL" in download_url[0] and "downloadURL" not in download_url[0]):
                download_url = download_url[0]['accessURL']

        check =   any(item in keyword for item in covid_keys[0].values.tolist())
        if (check == True):
            df = df.append({'Keyword':keyword, 'downloadURL': str(download_url)}, ignore_index= True)

    df.drop(df[df['downloadURL'] == ('Not Found')].index, inplace = True)
    url_df = df["downloadURL"]
    for index, url in url_df.items():
        urllib.request.urlretrieve(url, "cdcData" + format(index) + ".csv")

    from sqlalchemy import create_engine
    engine = create_engine('postgresql://postgres:uttasarga@localhost:5433/cdc')
    con = engine.connect()
    print(engine.table_names())


   

run_etl = PythonOperator(
    task_id = 'egen',
    python_callable = egen_data_etl,
    dag = dag
)

# from sqlalchemy import create_engine
# engine = create_engine('postgresql://postgres:uttasarga@localhost:5433/cdc')
# con = engine.connect()
# print(engine.table_names())


# t3 = PostgresOperator(task_id='my_postgres_task',
#                       sql="INSERT INTO test VALUES (3, 69, 'this is a test!');",
#                       postgres_conn_id='postgres_default',
#                       autocommit=True,
#                       database="cdc",
#                       dag=dag)

run_etl