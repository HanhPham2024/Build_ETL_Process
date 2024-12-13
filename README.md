# Build_ETL_Process
Building_ETL_Process of data from many resources

ETL is a process of Extracting, Trasformation and Loading to the database. In real, we must collect data from many other resources at first.
So, in this project, we will explore how to do it! 

At first, we must to import the nessesary libraries:
```python
import pandas as pd
import numpy as np


"Next, we will proceed with the ETL process as outlined below."

PAY ATTENTION!
_Looking at carefully in install connector, import connector_  

#Import creat_engine from the library sqlalchemy

from sqlalchemy import create_engine


# Module connector installation
**!pip install pymysql**
(the current project for Windows. For Linux, use code **pip3 install pymysql** for installation) 


# import connector

import pymysql




GO ON!!!



#The 1st file_ extract file df_work_experience
df_work_experience=pd.read_csv('work_experience.csv')

#The 2nd file_extract file df_Enrollies
df_Enrollies=pd.read_excel('enrollies_education.xlsx')

df_Enrollies.head()

df_City_development=pd.read_html('https://sca-programming-school.github.io/city_development_index/index.html')


#The 3rd file_extract file df_Enrollies_data
df_City_development=df_City_development[0]

print(df_City_development)

google_sheet_id= '1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI'
url='https://docs.google.com/spreadsheets/d/'+ google_sheet_id +'/export?format=xlsx'




#The 4th file_extract file df_Enrollies_data
df_Enrollies_data=pd.read_excel(url)

#Import creat_engine from the library sqlalchemy
from sqlalchemy import create_engine


# Module connector installation
!pip install pymysql

# import connector

import pymysql

driver='mysql+pymysql'
id= 'etl_practice'
url= driver+ '://' + id + ':550814@112.213.86.31:3360/company_course'

engine = create_engine(url)

#The 5th file_extract file df_employment
df_employment=pd.read_sql_table('employment', con=engine)

#The 6th file_extract file df_training_hours
df_training_hours=pd.read_sql_table('training_hours', con=engine)

# Transformation



# The df_employment
df_employment.head()

df_employment.info()

df_employment=df_employment.astype('int64')

df_employment.info()

#the df_training_hours
df_training_hours.head()

df_training_hours.info()

#the df_work_experience
df_work_experience.head()

df_work_experience.info()

df_work_experience=df_work_experience.convert_dtypes()

df_work_experience.info()

df_work_experience['experience'].describe()

mode_exp=df_work_experience['experience'].mode()[0]
mode_size=df_work_experience['company_size'].mode()[0]
mode_type=df_work_experience['company_type'].mode()[0]
mode_last_new_job=df_work_experience['last_new_job'].mode()[0]

df_work_experience['experience']=df_work_experience['experience'].fillna(mode_exp)
df_work_experience['company_size']=df_work_experience['company_size'].fillna(mode_size)
df_work_experience['company_type']=df_work_experience['company_type'].fillna(mode_type)
df_work_experience['last_new_job']=df_work_experience['last_new_job'].fillna(mode_last_new_job)



df_work_experience.info()

#the df_City_development
df_City_development.head()

df_City_development.info()

df_City_development=df_City_development.convert_dtypes()

df_City_development.info()

# the df_Enrollies
df_Enrollies.head()

df_Enrollies.info()

df_Enrollies=df_Enrollies.convert_dtypes()

#Consistence of df_Enrollies

df_Enrollies['enrolled_university']=df_Enrollies['enrolled_university'].str.lower()
df_Enrollies['education_level']=df_Enrollies['education_level'].str.lower()
df_Enrollies['major_discipline']=df_Enrollies['major_discipline'].str.lower()


#fill na
mode_enrolled_un=df_Enrollies['enrolled_university'].mode()[0]
mode_education_level=df_Enrollies['education_level'].mode()[0]
mode_major_discipline=df_Enrollies['major_discipline'].mode()[0]

df_Enrollies['enrolled_university']=df_Enrollies['enrolled_university'].fillna(mode_enrolled_un)
df_Enrollies['education_level']=df_Enrollies['education_level'].fillna(mode_education_level)
df_Enrollies['major_discipline']=df_Enrollies['major_discipline'].fillna(mode_major_discipline)

df_Enrollies.info()

df_Enrollies.head()

# the df_Enrollies_data
df_Enrollies_data.head()

df_Enrollies_data.info()

df_Enrollies_data=df_Enrollies_data.convert_dtypes()

df_Enrollies_data.info()

#fill na
mode_gender=df_Enrollies_data['gender'].mode()[0]
df_Enrollies_data['gender']=df_Enrollies_data['gender'].fillna(mode_gender)

df_Enrollies_data.info()

#Load the data

#Creat the connecting to database
#path to the SQLite database
db_path = 'data_warehouse.db'

#creat an engine

data_warehouse_engine = create_engine(f'sqlite:///{db_path}')




# Write DataFrames to database
df_work_experience.to_sql('Fact_work_experience_HP', con=data_warehouse_engine, if_exists='replace', index=False)
df_Enrollies.to_sql('Dem_Enrollies_HP', con=data_warehouse_engine, if_exists='replace', index=False)
df_City_development.to_sql('Dem_df_City_development_HP', con=data_warehouse_engine, if_exists='replace', index=False)
df_Enrollies_data.to_sql('Dem_df_Enrollies_data_HP', con=data_warehouse_engine, if_exists='replace', index=False)
df_employment.to_sql('Dem_employment_HP', con=data_warehouse_engine, if_exists='replace', index=False)
df_training_hours.to_sql('Dem_df_training_hours_HP', con=data_warehouse_engine, if_exists='replace', index=False)
