CONTENTS OF THIS FILE
---------------------

 * Introduction and Hypothesis
 * Approach
 * Findings


INTRODUCTION
------------

In VaderPlaysNBA, I attempt to answer the following question: Can historical Tweets that are represented as sentiment data be used to predict the winners of individual NBA games? As is evident from the problem statement, my primary source of data are Tweets. Additionally, I use ESPN’s historical records of individual NBA teams’ season schedule results in order to determine the outcome of individual games and desired time frames for scraping tweets per game. My hypothesis before beginning this project is that due to the informal and spontaneous often nature of Twitter and consequent confusion by the sentiment analyzer, my resulting model would not have a weighted f1 score of greater than 50%. Explained further, before beginning the project I was confident that a Tweet acquired by a search for “Lakers,” for example, which said something negative about the team that the Lakers were playing the next day, would result in a negative sentiment associated with the Lakers themselves, not the opposing team. Regardless, I was very excited to get started as this project is the intersection of a variety of topics that I am very interested in.

APPROACH
------------

than 50 Tweets about the Lakers from the night before each game, my collection of 50 Tweets before each game was only taken from about a 20 minute time frame between 11:40:00 until 11:59:59 pm--obviously not the random sample that I would have liked.

Next came the most painstaking part of the entire project. In order to get a list of tuples (start_date,end_date) for each game in the first half of the Lakers season, I had to manually copy dates, as YYYY-MM-DD, by referring to the table on the ESPN website referenced above. After finishing this step, my 50 Tweets from the night before each of the 41 games were now stored as html links in 41 .txt files. I was then able to read each .txt file and store these data as a list of lists in which each of the 41 sublists represented one game as 50 Tweet IDs.

Now that I had a list of “games” each represented as Tweets (still only Tweet IDs in the form of html links), I needed to store my data in a pandas DataFrame for visualization as well as modeling. So, before extracting the text from each of my Tweet IDs, I created an empty DataFrame for the Lakers 2018-2019 season with empty columns as sentiment analysis metrics (positive, negative, neutral, compound) and win or loss (W/L) of each game. Then, I used pandas html functionality and the python library requests to import the win loss column from the html ESPN Lakers table into my W/L column. I then made a final column that broke the win loss binary classification into ones and zeros (1 for win, 0 for loss).

After getting all of my Tweet IDs and creating a DataFrame ready for population, I used Tweepy and NLTK’s sentiment analysis tool Vader to get the average sentiment (4 metrics: pos, neg, neu, comp) of the 50 Tweet’s from the night before each game and load them into my DataFrame.

Before modeling, I repeated the same process with the Detroit Pistons ultimately resulting in another DataFrame of the same size with the same column labels etc. I chose the Pistons for my second team because they are in the Eastern Conference of the NBA (Lakers are in the Western), and they had a nearly 50-50 win loss record for the 2018-2019 season. Next, I combined concatenated these two DataFrame resulting in a final DataFrame with 4 sentiment analysis metrics and the outcome for 82 NBA games from the 2018-2019 season. Finally, I took a rather manual approach to modeling which was tedious but I ended up testing 20 different models with various models (Linear Regression, Random Forest, K-Nearest Neighbors 3, K-Nearest Neighbors-5) and five different sentiment features (pos, neg, neu, comp, four). The “four” feature was just an average of the original four sentiment metrics to see if this could do a better job. I used Scikit-learn to train and test these 20 models and created a new DataFrame with the model performances as f1 default score, f1 weighted score, and accuracy score.

FINDINGS
------------
I found the best model and feature combination to be a K Nearest Neighbors classification with all four sentiment features and k=5. This “K Nearest Neighbours (5) Four” as I called it performed with a weighted f1 score of 0.6203 or 62.03%. My reaction to this was that I was quite shocked that any of my models had performed above 50%. Additionally, it was interesting that using all four sentiment measures as features was the most effective. I concluded this version of the project with plotting a precision-recall curve of the “K Nearest Neighbours (5) Four” model as seen in the notebook.


Finally, I plan on improving and building on this project in the future by scraping and modeling data from all 82 games and all 30 teams in the NBA for one season. I plan to use pandas html functionality along with the datetime python library’s timedelta function to streamline what is currently the most tedious part of this process.
