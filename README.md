# FIFA 24 Stats Dataset Analysis

- [Overview](#overview)
- [Dataset Information](#dataset-information)
  - [How to Download the Dataset](#how-to-download-the-dataset)
- [Tools Used](#tools-used)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Analysis Highlights](#analysis-highlights)
- [Limitation](#limitations)



## Overview
This repository contains a comprehensive analysis of the FIFA dataset, exploring various aspects such as player demographics, distribution, and correlations between different attributes.

## Dataset Information

The analysis is based on the [FIFA 24 Player Stats Dataset](https://www.kaggle.com/datasets/rehandl23/fifa-24-player-stats-dataset) sourced from Kaggle. The dataset provides detailed information about football players around the world, including various attributes such as player,country,club, age, height, weight, skills, and more.

### How to Download the Dataset

1. Visit the [FIFA 24 Player Stats Dataset](https://www.kaggle.com/datasets/rehandl23/fifa-24-player-stats-dataset) page on Kaggle.

2. Log in to your Kaggle account or sign up if you don't have one.

3. Click on the "Download" button on the Kaggle dataset page to download the dataset as a ZIP file.


## Tools Used

The analysis was conducted using the following tools and libraries:

1. **[Python](https://www.python.org/downloads/):**
   - The analysis is primarily implemented using Python, a versatile programming language for data analysis and machine learning.

2. **[Jupyter Notebooks](https://www.anaconda.com/products/distribution):**
      - Jupyter Notebooks come as part of the Anaconda distribution. You can download Anaconda, which includes Jupyter, Python, and many data science libraries.
      - Jupyter Notebooks were used for developing and presenting the analysis. These interactive notebooks allow a combination of code, visualizations, and explanations.

    

4. **[Pandas](https://pandas.pydata.org/getting_started.html):**
   - Pandas was utilized for cleaning, preprocessing, and exploring the dataset.
   - Pandas is included in the Anaconda distribution, but you can also install it separately using pip:
     ```
     pip install pandas
     ```

5. **[Plotly](https://plotly.com/python/getting-started/):**
   - Plotly is a powerful visualization library for Python. You can install it using pip:
     ```
     pip install plotly
     ```

6. **[NumPy](https://numpy.org/install/):**
   - NumPy was used for numerical operations and array manipulation.
   - NumPy is included in the Anaconda distribution. You can install it separately using pip:
     ```
     pip install numpy
     ```

## Data Cleaning and Preparation

Before diving into the analysis, the dataset underwent a thorough cleaning and preparation process to ensure the accuracy and consistency of the results. The following steps were taken:

1. **Handling Missing Values:**
   - Identified and addressed missing values in key columns. Found out that the 'marking' column had all 'null' values thus removed the whole column
     ```python
     stats = stats.drop('marking',axis=1)
  
    
2. **Data Type Conversion:**
   - In the dataset the 'value' column was dtype-object but it contained numerical values. Therefore had to change it to dtype-float.
     ```python
     # The numerical values in the 'value' column had the '$' sign and '.' character.
     #First we remove the '$' sign and '.' before converting it to dtype-float 
      stats['value'] = stats['value'].str.replace('$','')
      stats['value'] = stats['value'].str.replace('.','')
     
     # Lets convert to float type
      stats['value'] = stats['value'].astype(float)
     
    - After changing the data type, I changed the column name from 'value' to 'value($)' for readability
      ```python
      # Lets change the column name from 'value' - 'value($)'
      stats = stats.rename(columns={'value': 'value($)'})
      
         
3. **Removing Duplicates:**
   - Checked for and removed any duplicate entries to maintain data integrity. Found that the dataset had 3 duplicated rows which I removed.
     ```python
     # Lets drop the duplicated values
      stats = stats.drop_duplicates()
     
     
4. **Feature Engineering:**
   - Created a new column to enhance the dataset for more insightful analyses.
     ```python
     # add skill average column by calculatng average of relevant columns

     # list of columns to exclude from average calcs
      exclude_col = ['player','country','height','weight','age','club','value($)']
      
     # columns to include in the average calculations to
      cols_to_average = [col for col in stats.columns if col not in exclude_col] # list comprehension for new list
      
     # calculate average for each player and assign to new 'rating' column
      stats['rating'] = stats[cols_to_average].mean(axis=1)
      
     # Rescaling rate to be out of 100
      max_rating = stats['rating'].max()
      min_rating = stats['rating'].min()
      
      stats['rating'] = 100 * (stats['rating'] - min_rating) / (max_rating - min_rating)

## Analysis Highlights

### 1. Distribution of Players' Height, Age, and Weight
Explored the distribution of players' height, age, and weight, providing insights into the physical attributes of football players in the dataset.

### 2. Top 10 Countries with the Most Number of Players
Identified and analyzed the top 10 countries with the highest number of football players in the dataset.

### 3. Distribution of Players Worldwide
Visualized the geographical distribution of players worldwide, showcasing the representation of different countries in the dataset.

### 4. Top 10 Clubs with the Most Number of Players and Most Valuable Players
Analyzed the clubs with the most significant player presence and identified the top players based on market value.

### 5. Correlation between Age and Skill
Investigated the correlation between players' age and their skill levels, providing insights into the relationship between age and performance.

### 6. Correlation between Different Skills
Explore correlations between various football skills, uncovering patterns and relationships between different attributes.

### 7. Top 10 Most Valuable and Least Valuable Players
Identified the players with the highest and lowest market values, offering insights into the valuation of players in the dataset.

## Limitations

While conducting this analysis, it's important to note certain limitations:

1. **Data Quality:**
   - The analysis is dependent on the quality of the original dataset. Inaccuracies or biases in the data may impact the robustness of the findings.

2. **Scope of Analysis:**
   - The analysis focuses on specific aspects of the FIFA dataset. Some nuances or comprehensive insights may not be captured due to the chosen scope.

3. **Market Value:**
   - Market values of players are subject to change, and the analysis may not reflect real-time valuations.


