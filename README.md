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
### Basic Descriptive Stats
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

The other goal of this part is to obtain the distribution of released_year and artist_count.
This can be achieved using the "df.plot.hist" function which purpose is to plot distributions of data.  
Distribution of Released Year:
``` python
plt.figure(figsize=(14, 5))
main['released_year'].plot.hist(color='hotpink', edgecolor='magenta')
plt.title("Distribution of Released Year")
plt.xlabel("Released Year")
plt.ylabel("Frequency")
plt.show()
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/03b32c14-daa5-42d0-ae69-f56fc1f0acbf)

Distribution of Artist Count:
``` python
plt.figure(figsize=(14, 5))
main['artist_count'].plot.hist(color='hotpink', edgecolor='magenta')
plt.title("Distribution of Artist Count")
plt.xlabel("Artist Count")
plt.ylabel("Frequency")
plt.show()
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/2d892893-d73f-4756-9163-b3773d8aa77d)  

---
### Top Performers
The first goal of this part is to present the track with the most streams and display the top 5 streamed tracks.  
To obtain the track with the highest amount of streams, we can use the function "df.nlargest()".  
in the line "(1,'streams')", 1 indicates the number of rows to be obtained. Where "streams" is the column to be evaluated.  
The parameter in the double brackets are the columns in which are to be kept and saved to the variable.
``` python
top_stream = main.nlargest(1,'streams')[['track_name', 'artist(s)_name', 'streams']]
top_stream
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/94002462-680c-455c-8c7a-0a717d5afe9e)

``` python
top5_streams = main.nlargest(5,'streams')[['track_name', 'artist(s)_name', 'streams']]
top5_streams
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/0c5ab0c1-404e-4cc5-b3e2-d236b0729fe8)
<br/>
<br/>
The other goal of this part is to find and display the top 5 artists that appear the most based on the number of tracks they have in the dataset.
This time we need to evaluate the column "artist(s)_name" and display the top 5 artists that appear the most.
To count the number of times each artist appears, we can us the function "df.value_counts()".   
Similarly to the previous question, we can get the top 5 values of the column using the "df.nlargest(5)" function.  
To reset the index on the side, we can use the function "df.reset_index()"
``` python
top5_most_artist = main['artist(s)_name'].value_counts().nlargest(5).reset_index()
top5_most_artist
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/1ca1e810-25eb-4f6c-a41e-4f12be6fd674)

<br/>

---
### Temporal Trends
The first goal in this part is to plot the number of tracks released per year.  
This time, the column "released_year" is the coulmn to be evaluated.  
<br/>
The first step we need to do is to count the numbers of time a year appears in the column "released_year". This can be done with the function "df.calue_counts()" where it counts the amount of times an element appears within its parameters where in this case "released_year".  
<br/>
Next we need to sort the years by increasing. We can do this with the function "sort_index()".  
<br/>
With this, we can now visualize the data using the function "plt.plot()" and to make it into a bar graph, we can use the parameter "kind='bar'".
<br/>
Now we need to label the graph properly. We can change the title using "plt.title()", the x axis label using "plt.xlabel()", and the y axis label with "plt.ylabel()".
``` python
plt.figure(figsize=(14, 5))
main['released_year'].value_counts().sort_index().plot(kind='bar', color='hotpink', edgecolor='darkmagenta')
plt.title('Number of songs released per year')
plt.xlabel('Year')
plt.ylabel('Number of songs')
plt.show()
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/ce36b13f-7a67-4f9f-84af-b97f90751a40)

<br/>
The next goal of this part is to display the amount of tracks released per month and which month has the most releases  
<br/>
This part is similar to the last part we did except this time, the column to be evaluated is "released_month".  
<br/>
Also this time, we need to sort the values inside the column so that we can project the month with the most releases to the least releases.  
This can be achieved with the function ".sort_values()" instead of the function ".sort_index()".

``` python
plt.figure(figsize=(14, 5))
main['released_month'].value_counts().sort_values(ascending=False).plot(kind='bar', color='hotpink', edgecolor='darkmagenta')
plt.title('Number of songs released per month')
plt.xlabel('Number of the Month')
plt.ylabel('Number of songs')
plt.show()
```
Where the output will be:  
![image](https://github.com/user-attachments/assets/7403c6c2-345f-4291-a95f-0fdb5d6fb773)

<br/>

---
### Genre and Music Characteristics
For this part we need to examine correlations of streams, bpm,  danceability_%, energy_%, valence_%, and acousticness_%.  
First we need to create a table that shows the correlation of each of the columns stated above.  
This can be done by using the function "df[["columns"]].corr()" where .corr() initiates getting the correlation.  
``` python
correl = main[['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].corr()
correl
```

Where the output is:  
![image](https://github.com/user-attachments/assets/5cc9fd4a-685f-4985-b404-b8f712d2aac0)

<br/>

With the table now made, we can now plot the data on a graph appropriatly a heatmap.  
<br/>
This can be done using the function "plt.imshow()" which creates a table with a square that represents each element of the table.  
The parameter "cmap='coolwarm'", changes the color of the squares where the more red a square is, the higher correlation each element has.  

``` python
plt.imshow(correl, cmap='coolwarm')
plt.xticks(range(len(correl.columns)),correl.columns, rotation=90) 
plt.yticks(range(len(correl.columns)), correl.columns)
```

Where the output is:  
![image](https://github.com/user-attachments/assets/9d7a19f9-f704-48c9-a9dd-5a7c096af4eb) 

<br/>

We also now need to get the correlation between danceability_% and energy_%, and valence_% and acousticness_%.  
This can be done just by indexing the table we first created holding the correlations between all the elements.  

``` python
correl_dance_energy = correl.loc['danceability_%', 'energy_%']
print("The correlation of danceablity and energy:", correl_dance_energy)
```

Where the output is:  
![image](https://github.com/user-attachments/assets/3a6717e0-3f1b-49e5-a9f3-a2c52056e546)

``` python
correl_valence_acousticness = correl.loc['valence_%', 'acousticness_%']
print("The correlation of valence and acousticness:", correl_valence_acousticness)
```

Where the output is:  
![image](https://github.com/user-attachments/assets/a45546ea-5db7-4c64-9c97-de0ea052c776)  

<br/>

---

###  Platform Popularity
For this part, we need to compare the number of tracks on the platforms; spotify_playlists, deezer_playlists, and apple_playlists.  
First, we need to address an issue in the column "in_deezer_playlists". Without the line "main['in_deezer_playlists'] = pd.to_numeric(main['in_deezer_playlists'], errors='coerce')", the sum of tracks would result to a extremly large number.  
<br/>
With that out of the way,  we now need to create a dataframe which contains the total number of tracks in that specific column.  
Here I first created a dictionary with the key as the column name and the value with the sum of its respective row.  
``` python
main['in_deezer_playlists'] = pd.to_numeric(main['in_deezer_playlists'], errors='coerce')
platform_count = { 'Spotify playlist': main['in_spotify_playlists'].sum(), 
                   'Deezer playlist': main['in_deezer_playlists'].sum(),
                   'Apple playlist': main['in_apple_playlists'].sum()}
```

With the dictionary made, we can now transform the dictionary into a dataframe using the function "pd.DataFrame()". The first parameter indicates the data, the dictionary, to be used and placed in the data frame, the second parameter labels the columns of the data frame respectively.

``` python
platform_count = pd.DataFrame(platform_count.items(), columns = ['platform', 'Number of tracks'])
```

With the dataframe created, we can now move on to plotting the dataframe with matplotlib.  
We can use the function "plt.bar()" with the column "platform" on the x axis and the column "Number of tracks" on the y axis.  
Then label the graph properly with the functions "plt.xlabel()", "plt.ylabel", and "plt.title".
``` python
plt.figure(figsize=(14, 5))
plt.bar(platform_count['platform'], platform_count['Number of tracks'], color='hotpink', edgecolor='magenta')
plt.title('Number of tracks of each Platform')
plt.ylabel('Number of tracks')
plt.xlabel('\nPlatform name')
plt.show()
```

Where the output is:  
![image](https://github.com/user-attachments/assets/fe403e46-615c-485b-a5a1-a1c2b8edf67d)

---

### Advanced Analysis
For the first part, we need to get the total number of streams of the same key and mode.
``` python
strms_by_key = main.groupby('key')['streams'].sum().sort_values(ascending=False).reset_index()
strms_by_key.columns = ['Key', 'Average Streams']
plt.figure(figsize=(14, 5))
plt.bar(strms_by_key['Key'], strms_by_key['Average Streams'], color='hotpink', edgecolor='magenta')
plt.title('Number of streams by key')
plt.ylabel('Number of streams')
plt.xlabel('\nKey')
```
Where the output is:  
![image](https://github.com/user-attachments/assets/a8852b5e-e811-4d96-ad7c-01f7db17364e)


``` python
strms_by_mode = main.groupby('mode')['streams'].sum().reset_index()
strms_by_mode.columns = ['Mode', 'Average Streams']
plt.figure(figsize=(14, 5))
plt.bar(strms_by_mode['Mode'], strms_by_mode['Average Streams'], color='hotpink', edgecolor='magenta')
plt.title('Streams by mode')
plt.ylabel('Number of streams')
plt.xlabel('\nMode')
```
Where the output is:  
![image](https://github.com/user-attachments/assets/29606883-8f6a-4e2a-81a0-a80109060bdd)

<br/>
<br/>

For the second part we need to display the top 10 artist the appear the most frequently in the playlists and charts respectively.  

``` python
main['total_playlist'] = main['in_spotify_playlists'] + main['in_apple_playlists'] + main['in_deezer_playlists']
main['total_chart'] = main['in_spotify_charts'] + main['in_apple_charts'] + main['in_deezer_charts']
artist_analysis = main.groupby('artist(s)_name')[['total_playlist', 'total_chart']].sum().reset_index()
artist_analysis = artist_analysis.sort_values(by=['total_playlist', 'total_chart'], ascending=False)
top_10_artists = artist_analysis.head(10)
plt.figure(figsize=(16, 5))
plt.bar(top_10_artists['artist(s)_name'], top_10_artists['total_playlist'], label='Playlist Appearances', color='hotpink',edgecolor='magenta')
plt.title('Top 10 Artists that appear in playlists')
plt.ylabel('Number of appearances')
plt.xlabel('Artist')
plt.show()
```
Where the output is:  
![image](https://github.com/user-attachments/assets/bca1d1df-07a8-4aff-98bc-964748e84665)


``` python
plt.figure(figsize=(14, 5))
plt.bar(top_10_artists['artist(s)_name'], top_10_artists['total_chart'], label='Chart Appearances',color='hotpink',edgecolor='magenta')
plt.title('Top 10 Artists that appear most in charts')
plt.ylabel('Number of appearances')
plt.xlabel('Artist')
plt.show()
```
Where the output is:  
![image](https://github.com/user-attachments/assets/59360a53-a3df-48f8-9e18-ad7cf861ae05)

---
## [History / Commits](https://github.com/DeiGarsya/Garcia_2ECE-A_EDA/commits/main/)

