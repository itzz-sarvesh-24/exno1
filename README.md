# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding
```
import pandas as pd
import numpy as np
from scipy import stats

def initial_data_info(df):
    print("Info:")
    print(df.info())
    print("\nDescribe:")
    print(df.describe())
    print("\nNull values per column:")
    print(df.isnull().sum())
    print('=' * 40)

def remove_nulls(df):
    return df.dropna()

def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower) & (df[col] <= upper)]
    return df

def remove_outliers_zscore(df, columns, thresh=3):
    for col in columns:
        z = np.abs(stats.zscore(df[col].dropna()))
        filtered_entries = (z < thresh)
        valid_indices = df[col].dropna().index[filtered_entries]
        df = df.loc[valid_indices]
    return df

file_names = [
    "Data_set.csv",
    "heights.csv",
    "iris.csv",
    "Loan_data.csv",
    "SAMPLEIDS.csv"
]

for file in file_names:
    print(f'Processing file: {file}')
    df = pd.read_csv(file)
    
    print("Initial Info")
    initial_data_info(df)
    
    df_no_nulls = remove_nulls(df)
    print("After Removing Nulls")
    print(df_no_nulls.shape)
    
    numeric_cols = df_no_nulls.select_dtypes(include=[np.number]).columns.tolist()
    
    df_iqr = remove_outliers_iqr(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (IQR method):", df_iqr.shape)
    
    df_z = remove_outliers_zscore(df_no_nulls.copy(), numeric_cols)
    print("After Removing Outliers (Z-score method):", df_z.shape)
    print('=' * 80)
```

# Output



<img width="1211" height="533" alt="Screenshot 2026-02-09 195249" src="https://github.com/user-attachments/assets/9625104e-6d57-40ef-8877-0ca4fa715307" />
<img width="1254" height="659" alt="Screenshot 2026-02-09 195226" src="https://github.com/user-attachments/assets/d0a9696d-d556-4900-a41b-1a362ebb3d51" />
<img width="1275" height="906" alt="Screenshot 2026-02-09 195152" src="https://github.com/user-attachments/assets/0fff3ea2-042e-4167-9bbe-cd6d93a7e1d0" />
<img width="1238" height="777" alt="Screenshot 2026-02-09 195139" src="https://github.com/user-attachments/assets/3cd6626f-6e44-466d-a2b8-27ea7650df7e" />
<img width="1019" height="901" alt="Screenshot 2026-02-09 195126" src="https://github.com/user-attachments/assets/98a6c60f-abfb-4dff-908d-a83897c44280" />
<img width="1146" height="903" alt="Screenshot 2026-02-09 195051" src="https://github.com/user-attachments/assets/f0cebadd-0951-4224-b6be-631fd9331b5d" />
<img width="1018" height="717" alt="Screenshot 2026-02-09 195035" src="https://github.com/user-attachments/assets/00ca3ad9-4e34-40b6-af9d-6c9b5912c19c" />
<img width="1033" height="850" alt="Screenshot 2026-02-09 195020" src="https://github.com/user-attachments/assets/a02eca7b-15b8-4b6f-bd99-7c37bfb1f1a1" />



# Result:
Thus the data cleaning process on the given dataset is executed successfully.
