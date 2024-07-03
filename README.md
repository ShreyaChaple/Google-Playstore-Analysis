# Google-Playstore-Analysis

<p>

## Overview

This project involves the analysis of a Google Playstore dataset to uncover insights about various apps. The dataset includes details such as app category, ratings, reviews, size, installs, type, price, content rating, genres, last updated date, current version, and required Android version.

## Requirements

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn

## Installation

To install the required packages, use the following command:
```bash
pip install pandas numpy matplotlib seaborn
```

## Data Loading

The dataset is loaded from a CSV file:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('/content/googleplaystore.csv')
df.head()
```

## Data Cleaning

### Removing Duplicates

The dataset contains duplicate entries which are removed:
```python
df.duplicated().sum()  # 483 duplicates
df.drop_duplicates(inplace=True)
```

### Handling Missing Values

Checking for missing values:
```python
df.isna().sum()
```

Columns with missing values include `Rating`, `Type`, `Content Rating`, `Current Ver`, and `Android Ver`. Missing values are handled as follows:
```python
df['Rating'].fillna(df['Rating'].mode()[0], inplace=True)
df['Type'].fillna(df['Type'].mode()[0], inplace=True)
df['Current Ver'].fillna(df['Current Ver'].mode()[0], inplace=True)
```

### Converting Data Types

Converting the `Reviews` column to numeric:
```python
df.Reviews = pd.to_numeric(df.Reviews, errors="coerce")
```

Processing the `Installs` column:
```python
def c(installs):
    if pd.isna(installs):
        return installs
    installs = str(installs)
    return installs.replace(',', '').replace('+', '')

df['Installs'] = df['Installs'].apply(c)
df['Installs'] = pd.to_numeric(df['Installs'], errors="coerce")
```

Stripping and converting `Current Ver` and `Android Ver` to numeric:
```python
df['Current Ver'] = df['Current Ver'].str.strip()
df['Current Ver'] = pd.to_numeric(df['Current Ver'], errors='coerce')

df['Android Ver'] = df['Android Ver'].str.strip()
df['Android Ver'] = pd.to_numeric(df['Android Ver'], errors='coerce')
```

### Handling Price Column

Cleaning the `Price` column:
```python
df.loc[df["Price"] == 'Everyone']
df.drop([10472], inplace=True, axis=0)
df.Price = df.Price.str.replace("$", "").astype(float)
```

## Analysis

### Distribution of Ratings

Visualizing the distribution of app ratings:
```python
plt.figure(figsize=(10, 10))
sns.countplot(x=df.Rating)
plt.show()
```

### Rating by Category

Visualizing the rating distribution across categories:
```python
plt.figure(figsize=(10, 10))
sns.barplot(x=df.Rating, y=df.Category)
plt.show()
```

## Conclusion

This analysis provides insights into the ratings, reviews, and other characteristics of apps on the Google Playstore. Further steps could include more advanced statistical analysis and machine learning models to predict app success based on the available features.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- The dataset used in this project is sourced from [Kaggle](https://www.kaggle.com).

</p>
