# Phase 1 Project



## Project Overview


### Business Problem

Microsoft sees all the big companies creating original video content and they want to get in on the fun. They have decided to create a new movie studio, but they don’t know anything about creating movies. You are charged with exploring what types of films are currently doing the best at the box office. You must then translate those findings into actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create.

### The Data

In the folder `zippedData` are movie datasets from:

* [Box Office Mojo](https://www.boxofficemojo.com/)
* [IMDB](https://www.imdb.com/)
* [Rotten Tomatoes](https://www.rottentomatoes.com/)
* [TheMovieDB](https://www.themoviedb.org/)
* [The Numbers](https://www.the-numbers.com/)

Three datasets were used 

[Box Office Mojo](https://www.boxofficemojo.com/)
[IMDB](https://www.imdb.com/)
 [The Numbers](https://www.the-numbers.com/)


# Loading the datasets
tn_data = pd.read_csv("zippedData/tn.movie_budgets.csv.gz", index_col=0)
imdb_data = pd.read_sql("""SELECT * FROM movie_basics  JOIN movie_ratings USING(movie_id);""", conn)
bom_data = pd.read_csv("zippedData/bom.movie_gross.csv.gz", index_col=0)

## Data Cleaning
### Box office Mojo Dataset
bom_data.drop(columns=["foreign_gross"], inplace=True)
bom_data[bom_data["foreign_gross"].isna()]

### IMDB Dataset
imdb_data.isna().sum()
imdb_data["genres"].fillna("unknown", inplace=True)
dups= imdb_data.duplicated().any().sum()

### The Numbers Dataset

#### How to strip dollar signs and the commas first
#use string method to replace dollar and comma with nothing
tn_data['domestic_gross'] = tn_data['domestic_gross'].str.replace('$', '').str.replace(',', '')
tn_data['production_budget'] = tn_data['production_budget'].str.replace('$', '').str.replace(',', '')
tn_data['worldwide_gross'] = tn_data['worldwide_gross'].str.replace('$', '').str.replace(',', '')


### Merging the Datasets

merged_data = imdb_data.merge(bom_data, on=['title', 'year']).merge(tn_data, on=['title', 'year'])



# Visualizations

### Checking the Genres that are popular in rating

![Image 1](https://user-images.githubusercontent.com/130972835/232905728-11b0dd92-04c4-46db-9349-2ea1fef5547c.jpeg)
### Checking and comparing Runtime vs rating
![image 2](https://user-images.githubusercontent.com/130972835/232924108-ef0eaf47-3ac1-484f-852a-5e7670f18d1e.jpeg)
## Production budget vs worldwide gross

![image 3](https://user-images.githubusercontent.com/130972835/232924203-a793bea9-171a-4e07-b55e-a3a3beaa2662.jpeg)



Conclusions and recommendations
### Budget for best returns

100 million should be the goal if we want a good return, however, smaller projects can be done if they are able to hit the 7-8 rating, even though it has a weak positive correlation with both domestic and worldwide gross, 

### Genres to invest into for best ratings

Crime,Documentary          
Adventure,Drama,Sci-Fi     
Action,Drama               
Mystery,Thriller           
Action,Comedy,Drama        
Action,Sci-Fi              
Biography,Drama,Musical    
Adventure,Drama,Western    
Drama,History,Thriller     
Drama,Western              

### Runtime
-Rating has a strong positive correlation with the runtime of the movie, the dirstibution is clustered within a value range of 80 to 140 minutes, hence a runtime of that range will suffice for normal movies.

Documentaries may be longer though 
