# Netflix Movie Analysis

## 📌 Project Overview
This project analyzes a dataset of Netflix movies to uncover insights about movie popularity, genres, ratings, and release trends. The analysis includes data cleaning, transformation, visualization, and statistical exploration to understand what makes movies successful on the platform.

## 🎯 Project Goals
- **Explore movie characteristics:** Understand how popularity, vote counts, and ratings are distributed.
- **Analyze genre trends:** Identify which genres are most common and how they correlate with popularity.
- **Examine temporal patterns:** See how movie releases and performance have changed over time.
- **Build visualizations:** Create informative charts to communicate findings effectively.
- **Develop actionable insights:** Provide recommendations based on data patterns.

## 📂 Dataset Information
The dataset contains 9,827 movies with the following features:

| Feature       | Description                                                     |
| ------------- | --------------------------------------------------------------- |
| Release_Date  | Year of movie release (converted from full date)               |
| Title         | Name of the movie                                              |
| Popularity    | Numerical popularity score                                      |
| Vote_Count    | Number of user ratings                                         |
| Vote_Average  | Average user rating (categorized into: popular, average, below_avg, not_popular) |
| Genre         | Movie genre(s) (originally comma-separated, later exploded into individual rows) |

## 🛠️ Technical Implementation

### Data Processing Steps

**Initial Setup:**
- Uploaded dataset to Google Colab
- Imported necessary libraries (`pandas`, `numpy`, `matplotlib`, `seaborn`)

**Data Cleaning:**
- Converted `Release_Date` from string to datetime format and extracted year
- Removed unnecessary columns (`Overview`, `Original_Language`, `Poster_Url`)
- Checked for and handled missing values
- Verified no duplicate entries

**Feature Engineering:**
- Categorized `Vote_Average` into 4 meaningful groups:
  - `popular` (≥ 7.1)
  - `average` (6.0 - 7.0)
  - `below_avg` (4.0 - 5.9)
  - `not_popular` (< 4.0)
- Exploded `Genre` column to have one genre per row

**Exploratory Analysis:**
- Basic statistics and distributions
- Genre frequency analysis
- Temporal trends by release year
- Popularity vs. rating relationships

### Key Code Snippets
```python
# Convert release date to datetime and extract year
df['Release_Date'] = pd.to_datetime(df['Release_Date'])
df['Release_Date'] = df['Release_Date'].dt.year

# Categorize vote averages
def categorize_col(df, col, labels):
    edges = [df[col].describe()['min'],
             df[col].describe()['25%'],
             df[col].describe()['50%'],
             df[col].describe()['75%'],
             df[col].describe()['max']]
    df[col] = pd.cut(df[col], bins=edges, labels=labels, duplicates='drop')
    return df

labels = ['not_popular', 'below_avg', 'average', 'popular']
categorize_col(df, 'Vote_Average', labels)

# Explode genres
df['Genre'] = df['Genre'].str.split(',')
df = df.explode('Genre').reset_index(drop=True)
