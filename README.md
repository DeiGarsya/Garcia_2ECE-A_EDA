# Exploratory Data Analysis - Spotify 2023 Dataset
## Name: Deirojel Martin M. Garcia
## Section: 2ECE-A
---
## **General Guidelines**
1. Begin by familiarizing yourself with the structure of the dataset. Check for missing values and data types, and perform an initial exploration to understand the different features available.
2. Provide summary statistics to give an overview of key metrics such as the number of streams, release dates, and musical attributes (e.g., BPM, danceability).
3. Use appropriate visualizations (e.g., bar charts, histograms, scatter plots) to uncover trends and patterns in the data. Ensure that your plots are well-labeled and easy to interpret.
4. Investigate correlations between different variables and provide insights based on your findings. Explore relationships between streams and other musical characteristics like tempo, energy, or playlists.
5. Based on your analysis, offer any insights or recommendations regarding the tracks, artists, or musical trends that could be useful for understanding what makes a track popular.
---
## **Requirements and Submissions**
1. Create a GitHub repository that fully documents the exploratory data analysis on the given dataset.
  - Ensure that your code is clean, well-commented, and organized.
  - Use Python libraries such as pandas for data manipulation and matplotlib or seaborn for visualization.
  - Provide a brief written interpretation of each key insight you discover.
2. Your repository should include the Jupyter Notebook and Readme file. Your repository should be unique and not similar to another student in the batch. It should also not be copied from the internet. The similarity index should only be up to 10%.
---
## **Analysis/Coding**
---
### Importing and Setting up
Before starting the analysis, we need to import the required libraries to make analyzing the spotify dataset possible.
- Pandas - This library allows us to manipulate numerical data within a table.
- Matplotlib - This library allows us to create visualizations of data that we create and analyze using Pandas.
``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```
Now we can import the given spotify dataset "spotify-2023.csv". Since the file is encoded in utf-8 and cannot be loaded, it can be loaded in other encodings such as "latin-1".
``` python
main = pd.read_csv('spotify-2023.csv', encoding='latin-1')
main
```
---
### Overview of Dataset
The rows and columns of the dataset can be measure using the "df.shape()" function and can be assigned simultaneously to 2 variables.
``` python
rows, columns = main.shape
print("Number of Rows:", rows)
print("Number of Columns:", columns)
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/178e19a5-e891-4afb-a7dd-f8b97542d553)  
  
  
The datatypes and number of occupied rows of each column can be observed through the funcion df.info()
```python
main.info()
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/c2aef997-1c77-4444-a8b4-8b9504cd1a06)  

---
## Basic Descriptive Stats
The goal is to obtain the Mean, Median, and Standard Deviation of the column "streams" but since the datatype of the column "stream" is object, we first need to convert it into a numerical dtype.  
This can be achieved using the function "pd.to_numeric"
``` python
main['streams'] = pd.to_numeric(main['streams'], errors='coerce')
```
In the line "errors='coerce'" this means that if there is an error that occurs, the line while still force the conversion to continue. This is needed because the column "streams" contains a row with letters and we need to convert the dtype into a numerical dtype.  

Now we can get the Mean, Median, and Standard Deviation of the column "stream".  
We can use the "df.mean()", "df.median()", and "df.std()" functions to obtain these values.
```python
streams_mean= main['streams'].mean()
streams_median= main['streams'].median()
streams_std=main['streams'].std()

print("Mean of values in the column streams:", streams_mean)
print("Median of values in the column streams:", streams_median)
print("Standard deviation of values in the column streams:", streams_std)
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/a8e5b6f8-71d2-4e6d-b80c-dd4e2d5e4498)
