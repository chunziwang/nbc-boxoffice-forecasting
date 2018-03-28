# NBCUniversal: Predicting Which Movies will be a Hit or a Bust
----

## Business Problem

Box-office forecasting is a challenging but an important task for movie distributors in their decision making process.  The global film industry shows healthy projections for the coming years, as the global box office revenue is forecast to increase from about 38 billion U.S. dollars in 2016 to nearly 50 billion U.S. dollars in 2020.  However, box office revenue is down 10% so far from 2016.  The task is to leverage the data to help determine the unexpected behavior of movies, and address one or more of the following questions:

+ Why do some small budget films end up being blockbuster hits? Conversely, why do some large budget films fail?
+ Do certain genres lend themselves to higher return? Horror, romantic comedies, science fiction?
+ Do remakes, tent-poles and sequels perform differently?
+ How does the time of year, weather and economic trends influence box office performance?

## Dataset Overview

Excel spreadsheet with movie information such as release date, actors, budget, box office revenue, ratings, etc. It contains 8469 movies * 19 attributes. The attributes consist of:

- imdbid
- title
- plot
- rating 
- imdb_rating
- metacritic
- dvd_release
- production
- actors
- imdb_votes
- poster
- director
- release_date
- runtime
- genre
- awards
- keywords
- budget
- box office gross

## Data Cleaning and Processing

This dataset is very messy, in terms that most of the data are characters and have bunch of words together, and there're a lot of missing values. The main data cleaning and processing job I did here include:

- Remove plot and poster column because they are not needed here.
- Sort the movie by release date and put the movies that’re released after 2017 in a new set so we could predict the box office for them using historical box office value. (they have no budget and box office value in the dataframe now) 
- Remove the rows that have missing values in budget or box office. We'll only work with data that shows the movie has a concrete box office value and budget value. Fill in NAs that appear in other column with 0.
- Seperate release date into new columns - year, month, and date.
- Extract keyword column out from dataset for modeling use.
- Add a roi column.
- Detect and remove outliers.
- Transform some character columns into numberic columns such as runtime.
- For other categorical columns including rating, production, actors, director, genre, awards, keywords, we use bag of words method to create a document tern matrix and add these dummy variable to the cleaned dataset for modeling purpose.


## Exploratory Analysis

### Does good imdb rating positively correlated with good box office?

Yes. Movies that have achieved high box office gross tend to have higher ratings. But movies that have higher ratings don’t necessarily have high box office gross.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

### Relationship of budget and box office gross

When the budget is within reasonable amount, higher budget may bring more box office though not always the case.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

Observed which movies have high budgets and low box office. Most of them win lots of awards and nominations. Probably more artistic and only appeal to a niche market than a blockbuster.

### Do certain genre lend themselves to higher return?

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

Intersting and important Findings:

- Although there’re more than 300 segmented genres, it’s clear in the plot that they could be integrated into three main categories: comedy & romance & drama, horror & thriller & mystery, and action & adverture & crime. Sci-Fi and Documentary are in smaller amount so these two genres didn’t really stand out.

- In terms of box office gross, action > comedy > thriller. It makes sense that people like watching action movies because it’s exciting, and thriller is less relatable to the general public.

- In terms of roi, thriller > comedy > action. Action movie needs more resources to shoot and make the setting, while thriller is generally less cost-consuming.

### Relationship of director and box office

This plot is sort by number of movies each director has in the dataset, because we’re more interested in well-known directors who have produced many award-winning movies. But it also means directors who has less movies might be left out in this plot. Given the limitations of space we chose the top 30 directors based on the number of their works.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

It's noticed that directors including Steven Spielberg, Peter Jackson, and Michael Bay have comparatively high box office. Let's observe what type of movies those directors with higher box office shot to see if it's more of a genre issue or director issue.

Steven Spielberg : Jurassic Park series, ET, Schindler’s list, AI, Indiana Jones. Adventure, sci-fi, drama, war, history, action, biography.

Peter Jackson: Hobbit series and King Kong. Fantasy and adventure.

Michael Bay: Transformers series. Action.

James Cameron: Titanic, Avatar, terminator. Action, sci-fi, adventure.

This plot below shows the sum of box office of every director. This time it's sorted by the sum of box office instead of movie numbers. Directors who made it in this plot are the top 30.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

This plot is the average box office of for directors who have more than 3 movies in this dataset. James Cameron comes on top this time. He’s not as productive as spielberg, but he’s definitely the lucrative.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

Compare roi of different directors. James Wan comes top here.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

### Does timeline influence box office?

It seems that no matter past or present, every year has highs and lows. We didn't consider the inflation of currency here so it doens't mean movies nowadays bring in more money.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

About 200 movies are released every year since 1990s. To observe how time of the year influence box office, we choose year between 1988-2017 since it has more data. We plot the average of monthly movie box office of every year in scatterplot.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

This time I don’t use average monthly box office, I put the box office of every movie here. The results are similar: movies released in May, June, July (summer time) and December tends to have more box office. It makes sense.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

### Do remakes (adaptions), tent-poles, and sequels perform differently?

Fitst step, find sequels. Sort the titles by alphabetical orders and you’ll find the sequels: 21/22 Jump Street, A Huanted House, Alvin and the Chipmunks, American Pie… But it’s time-consuming. A clever way would be to group by director, since a series of movies are often filmed by the same director.

I chose the top 50 director on the director dataframe sorted by the number of their films on the dataset, and sort it so it’s easier to spot the sequels, especailly the well-known ones.

I saw Twilight Saga, X-Men, Fast and Furious, Transformers, the Hobbit, Back to the future, Spider Man, Madea series, the hangover, Star war series, Jurassic Park series here.

For adaptions, I saw Alice in Wonderland, Beauty and Beast, batman V superman, the three musketers, pride & prejudice here.

I selected some well-known movie franchise for review. it turns out that there're 3 Star Wars series, 3 Hobbit series, 7 Fast and Furious series, 4 Transformer series, 5 Spider series, 2 Harry Potter series, 3 Jurassic Park series, and 3 Batman series here in this dataset. 

I put all the infomation about release date, box office, and roi of every movie in these series in the below plot:

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

Some interesting findings:

- Generally speaking, the majority of series movies have roi above 0, but not all sereis movies make money, e.g. the latest Transformers and Fast & Furious. Actually the performance of series movies are quite unstable. So it's unadvisable to keep shooting the next series if the market impact keeps diminishing. 

- Jurassic Park and Star Wars keep roi above 2, it shows a good story, or story with a strong, long-lasting audience base have a longer "life time value" for production.

For adaptions and remakes, there’re 2 adapted from fairy tales (Alice in Wonderland, Beauty and Beast), and 2 from classic literature (the Three Musketers, Pride & Prejudice). The box office and roi seems to be related to the popularity of the content and imdb ratings. For some literary work that’s less well-known and may not be well produced, it’s natural that the roi is negative. So choosing the right Intellectual Property “IP” here is very important for remakes and adaptions, say Beauty and the Beast.

### Actor Analysis: Who is the money-spinner?

The Top 50 actors who appear in our dataset by number is Robert Deniro, Tom Hanks, Adam Sandler, Liam Neeson, Steve Carell and so on. There’re lots of big stars in this list including Robert Downey Jr, Julianne Moore, Matt Damon, Amy Adams, Jennifer Aniston, Johnny Depp, Arnold Schwarzenegger, Matthew McConaughey.

Due to the number of predictors is larger than the number of observations, we only include a limited number of actors here (keep the well-known ones to see if they have a direct association with box office) based on how many movies they played in in this dataset (>=10).

After fitting a linear regression model, we see there’re some actors with positive coefficients meaning positive impact on box office if they are in the movie, and some with negative coefficients. Some actors have p-value < 0.05, meaning they do make a difference, and more are not significant. We analyze the actors with significant influence here.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

All actors that have significant influence over box office have positive coefficients here. The top 5 actors who could bring considerable amount of box office are Vin Diesel, Harrison Ford, Robert Downey Jr, Jennifer Lawrence, and Tom Hanks. Generally speaking more actors than actresses make it to this graph, and this may have something to do with Hollywood’s unfair payment between gender.

### Does a movie’s rating influence its box office?

The three most common ratings are R, PG-13, PG here, so we’ll only analyze the difference among these three.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

R rating movies mostly have lower box office than PG-13 and PG. It’s easy to understand since higher restriction level limits the audience.

### Why do some small budge films end up being blockbuster hits? Conversely, why do some large budge films fail?

We filter out movies that have top 10 roi and bottom 10 roi to find a pattern.

By observation, it’s noticed that for movies that have high roi, they mostly have low budget, not-so-well-known director and actors, but have some nominations and awards. And half of them are horror+thriller genre.

For movies that have low roi, their budget is high and box office failed to make ends meet. Half of them are action movies and half are drama+romance. Action movie is usually more costly than other genre as we mentioned above.


## Predictive Modeling

I split the cleaned data into training and test set to do cross validation to check the predictive power of models. I chose to use linear regression, dynamic trees, random forest, and xgboost here. xgboost has the lowest rmse here.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

In linear regression, it shows that predictors including imdb rating, imdb votes, some actors such as Vin Diesel/Robert Downey Jr/Jennifer Lawrence, some directors such as James Cameron/James Wan/Michael Bay, genre such as adventure/drama, keywords such as death/female/nudity/blood/family/animal, production such as Universal Pictures/WarberBros.Pictures are significant to the box office result. In the residual plot it shows some pattern in the lower left corner and I'll look into that and investigate deeper in further exploration.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

This is the dynamic regression tree plot, it only used imdb votes and budget to split the tree, no wonder the prediction result is even not as good as linear regression.

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

This is the importance plot of random forest:

![](https://github.com/chunziwang/whole-foods-market-basket-analysis/blob/master/figs/1.png)

More work will be done after this initial modeling, such as tuning parameter for xgboost, and introducing more predictors such as population and inflation rate to predict the box office more accurately.