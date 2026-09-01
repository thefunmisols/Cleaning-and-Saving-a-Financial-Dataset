# Cleaning-and-Saving-a-Financial-Dataset Using Pandas
A structured data-cleaning workflow using pandas

In this hands-on activity, I practiced a structured data-cleaning workflow using pandas. I corrected formatting issues, fixed data types, handled missing values, and removed duplicates, then validated and exported a clean version of the dataset. I worked with a CSV file, a simple text-based format that pandas reads and writes easily. My task was to prepare the dataset for financial analysis—clean, consistent, and ready for use.

The objective was to use pandas to clean a messy financial dataset by applying formatting corrections, type conversions, missing-value treatment, and duplicate removal—then export a clean CSV file.

**Tools Used**

Python 3

pandas

Jupyter Notebook

Provided file: financial_transactions.csv (A CSV—comma-separated values—file stored in plain text. Pandas converts it into a DataFrame with read_csv() and saves cleaned data back with to_csv().)

**CLEANING PROCESS**

**1. I Set Up My Notebook**

Created a new notebook named clean_financial_dataset.ipynb.

Loaded pandas and read the CSV:

imported pandas as pd 

df = pd.read_csv("fin_transactions.txt")

This command loaded the CSV file—plain text with commas separating columns—into a pandas DataFrame. Pandas then treated it like a structured table you can inspect and clean.

I ran a quick initial snapshot:

df.head()
df.info()
df.isna().sum()

<img width="784" height="359" alt="yes1" src="https://github.com/user-attachments/assets/208c75de-e9f6-4a1c-adc8-97aea8929b75" />

<img width="752" height="437" alt="yes2" src="https://github.com/user-attachments/assets/a3eb3267-9fb4-4075-8d3f-ef823e88e53c" />


Some of the things that looked incorrect or inconsistent were;

Amount and Date stored as text

Missing values

Inconsistent amount formatting

Potentially unusual negative amount

2. I cleaned the Data

I have included in each cleaning step a brief comment explaining why I chose that action.

A. I removed duplicate rows

df.drop_duplicates(inplace=True) - discovered that there were no duplicates in the data from this by checking with df.shape

B. I fixed formatting for numeric fields

Stripped currency symbols and commas; converted the amount column from text to numeric, did this because calculations cannot be done when the amount column is not in the correct data type.

df["amount"] = (df["amount"]
        .astype(str)
        .str.replace(r"[\$,]", "", regex=True)
        .str.strip()
)
df["amount"] = pd.to_numeric(df["amount"], errors="coerce")

<img width="781" height="317" alt="yes4" src="https://github.com/user-attachments/assets/fd7f2f93-f786-49bf-bfa9-e438d365bd8e" />

C. I converted text-based dates into real datetimes using the function below because time series analysis wont be effectively done if date is not in the correct format.

df["date"] = pd.to_datetime(df["date"], errors="coerce")

D. I standardized category labels for reliable grouping and reporting;

df["category"] = df["category"].str.strip().str.lower()

<img width="783" height="356" alt="yes" src="https://github.com/user-attachments/assets/8e0b7fad-3762-411a-84c3-078254a9a60e" />

E. I handled missing values choosing carefully how based on context using the function below as handling missing values carelessly could affect the overall totals and distort financial metrics leading to wrong decisions.

df["amount"] = df["amount"].fillna(0)
df["missing_flag"] = df.isna().any(axis=1)

<img width="779" height="239" alt="fire" src="https://github.com/user-attachments/assets/08486b3f-f0b5-448a-ab44-6cbd882366fe" />

I considered whether dropping rows would harm financial accuracy before doing so, for the rest of the missing values(1 missing account_id and 2 dates, I would conduct further investigation to determine if they can be dropped.

3. I validated the cleaned Dataset by running the functions below;

df.info()

df.describe()

df.isna().sum()

<img width="749" height="427" alt="zt" src="https://github.com/user-attachments/assets/a9ab0db8-2149-40c5-8a30-a3cc4c1682e6" />

From the results I confirmed:

that the amounts were numeric

the dates had proper datetimes

the missing values made a bit of sense

all duplicates were gone(there weren't any duplicate values to begin with)

all categories were consistent

4. I saved my Cleaned Dataset with the function;
   
df.to_csv("financial_transactions_clean.csv", index=False)

This wrote my cleaned dataFrame back into a CSV file, again using commas to separate columns. 

**Skills gained**

In this activity, I

Cleaned a raw dataset using essential pandas techniques

Corrected numeric formatting

Converted columns to the proper data types

Identified and treated missing values

Standardized categories

Removed duplicate records

Validated results and exported a final dataset
