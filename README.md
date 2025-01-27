# NextGan-project

## Problem Statement

## 1. Descriptive Analysis

1. **Analyze the distribution of movie ratings. What percentage of movies have high (5), medium (3-4), and low (1-2) ratings?**
2. **Identify the top 10 most-rated movies**

## Solution

This project aims to analyze movie ratings data to understand the distribution of ratings and identify the most-rated movies. The solution involves the following steps:

### Data Loading

- Load the movie ratings data from `ratings.dat`.
- Load the movie information from `movies.dat`.

### Data Processing

- Classify each rating as high (5), medium (3-4), or low (1-2).
- Calculate the percentage of movies in each rating category.
- Count the number of ratings for each movie.
- Identify the top 10 most-rated movies.

### Results

- Rating Distribution: The percentage of movies with high, medium, and low ratings is calculated and displayed.
- Top 10 Most-Rated Movies: The top 10 movies with the highest number of ratings are identified and displayed.
### Code Implementation

```python
# First Solution - Descriptive Analysis

ratings = open(r"D:\for practice\ml-1m\ratings.dat")
rating_distribution = dict()
movieId_count = dict()

for line in ratings:
    line = line.strip()
    columns = list(map(int, line.split('::')))
    if columns[2] == 5:
        columns.append('High')
    elif columns[2] == 4 or columns[2] == 3:
        columns.append('Medium')
    else:
        columns.append('Low')

    if columns[1] in movieId_count:
        movieId_count[columns[1]] += 1
    else: 
        movieId_count[columns[1]] = 1

    if columns[4] in rating_distribution:
        rating_distribution[columns[4]] += 1
    else:
        rating_distribution[columns[4]] = 1

for rating_range in rating_distribution:
    print('{0} {1}'.format(rating_range, int(rating_distribution[rating_range] / sum(rating_distribution.values()) * 100)))

movies = open(r'D:\for practice\ml-1m\movies.dat')
movieId_name = dict()
for line in movies:
    line = line.strip()
    columns = line.split('::')
    movieId_name[int(columns[0])] = columns[1]
    
sorted_counted_data = sorted(movieId_count.items(), key = lambda x:x[1], reverse=True)[:10]
for movieId, count in sorted_counted_data:
    print(movieId_name[movieId], count)
```
## 2. Genre Insights

1. **Which movie genres are the most frequently rated?**
2. **Compare the average ratings across different genres. Are certain genres consistently rated higher or lower?**

## Solution

This project aims to analyze movie ratings data to gain insights into genre popularity and rating patterns. The solution involves the following steps:

### Data Loading

- Load the movie ratings data from `ratings.dat` and movie information from `movies.dat`.

### Data Processing

- Calculate the total number of ratings and the sum of ratings for each movie.
- Compute the average rating for each movie.
- Aggregate the ratings by genre to determine the total number of ratings and the sum of ratings for each genre.

### Analysis

- Identify the most frequently rated genres by counting the number of ratings for each genre.
- Calculate the average rating for each genre by dividing the sum of ratings by the number of ratings.
- Compare the average ratings across different genres to identify which genres are consistently rated higher or lower.

### Results

- Most Frequently Rated Genres: The genre with the highest number of ratings is identified and displayed.
- Top 10 High-Rated Genres: The top 10 genres with the highest average ratings are listed.
- Top 10 Low-Rated Genres: The top 10 genres with the lowest average ratings are listed.

### Code Implementation

```python
# filepath: /d:/for practice/Project_02.py
with open(r"D:\for practice\ml-1m\ratings.dat","r") as f, open(r"D:\for practice\ml-1m\movies.dat","r") as d : #UserID::MovieID::Rating::Timestamp
    rating_data = f.readlines()                                                                                #MovieID::Title::Genres
    movie_data = d.readlines()
    
MovieID_avg_count = dict() # this dict is use to stored the count and sum of each movie rating which rated by audiance
for line in rating_data:
    line = line.strip()
    columns = list(map(int, line.split('::')))
 

    if columns[1] in MovieID_avg_count:
        MovieID_avg_count[columns[1]][0] += columns[2]
        MovieID_avg_count[columns[1]][1] +=  1
    else: 
        MovieID_avg_count[columns[1]] = [0,0]
        MovieID_avg_count[columns[1]][0] = columns[2]
        MovieID_avg_count[columns[1]][1] =  1
    if columns[1] == 51:
        print(columns)
        

avg_of_rating = [round(MovieID_avg_count[i][0]/MovieID_avg_count[i][1], 2) for i in MovieID_avg_count]
movie_avg_rating = dict(zip(MovieID_avg_count.keys(), avg_of_rating)) # we assgin avg rating to ṃovieID



genres_rating_with_count = dict() # we crated the dict where we can stored the total rating and sum of all rating of each genres
for line in movie_data[:]:
    line = line.strip()
    columns = list(map(str, line.split('::')))
    columns[0] = int(columns[0])
 
    if columns[2] in genres_rating_with_count:
        if columns[0] in movie_avg_rating :
            genres_rating_with_count[columns[2]][0] += movie_avg_rating[columns[0]]
            genres_rating_with_count[columns[2]][1] +=  1 
            
    else :
        if columns[0] in movie_avg_rating :
            genres_rating_with_count[columns[2]] = [0,0]
            genres_rating_with_count[columns[2]][0] = movie_avg_rating[columns[0]]
            genres_rating_with_count[columns[2]][1] =  1


print("Most frequently rated Genres = ",max(genres_rating_with_count.items(),key=lambda x: x[1][1] ),"\n") 

avgrating_ = [round(genres_rating_with_count[i][0]/genres_rating_with_count[i][1], 2) for i in genres_rating_with_count]
Genres_avg_rating = dict(zip(genres_rating_with_count.keys(), avgrating_)) # we assgin avg rating to ṃovieID

Top10_high_rating = dict(sorted(Genres_avg_rating.items(),key=lambda x: x[1] ,reverse=True)[:10]) 
Top10_low_rating = dict(sorted(Genres_avg_rating.items(),key=lambda x: x[1] )[:10])

print("top 10 high rated Genres =",Top10_high_rating,"\n")
print("top 10 low rated Genres =",Top10_low_rating)
```
