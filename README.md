## Fraud Detection With Isolation Forest And Other Common Classifiers

In this project, I explore the implementation and evaluation of several algorithms used by financial institutions to monitor for credit/debit card fraud. This process used to be dominated by a rules-based methodology in which a consumer’s spending habits such as frequency of use, location, amount were monitored to see if their card activity deviated from a 'hard-coded set of rules. In fact, many consumers are used to the idea of needing to notify their bank or card issuer before making large purchases or traveling. I recall an incident where, after having my first credit card for a few months, needing to do some shopping after moving from my hometown to Los Angeles. After a day of trekking across the city to buy school supplies and household goods, I decided to treat a friend and myself to a nice dinner. To my shock and embarrassment, when I tried paying the server informed me that my card had been declined. Frantic, I checked my account using the mobile app and found nothing amiss. Fortunately, after a few minutes, I received a call from the company I had the card with asking me to confirm my identity and then to inquire about my spending that day after which my card was unlocked and I was able to to pay for the meal.

Today, financial institutions have a much more robust set of tools thanks t an array or of machine learning algorithms that can process massive amounts of transaction data to detect anomalies that would be otherwise extremely difficult or tedious to set up a rules schema for.

One of the most widely used is the isolation forest algorithm which makes use of a simple intuition. As an ensemble method, it randomly selects a feature to serve as the root of each decision tree and further splits the data based on random values taken from those features. By doing this a norm can be ascertained by finding the average depth required to isolate each point of data and computing an average depth. And since anonymous data does not trend with normal data, these transactions can be detected because their depths are outside of that norm.






### Data

This dataset is drawn from taken from [Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud) and contains 170885 rows of anonymized data represented by 31 numeric features including time, which I will drop as it is not required for this analysis, a binary target feature designated as 'Class', a feature representing dollar amount for each transaction. Rows with a value of 1, of which there are 360, represent fraud while 0 represents non-fraudulent transactions.



### Models


I chose to start my analysis with the [Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html) algorithm which is especially good at finding outliers. As an ensemble method, it randomly selects a feature for each tree's root and also selects a random value along the range of those features values to split upon. The intuition follows that an outlier to the data will require a much shorter path to its isolation, i.e. a pure leaf. The difference between the length of this path and the average depth required by normal data is then computed as the score.



### Initial Findings
While it seems I was able to achieve somewhat decent results from the model, confusion matrices are not well suited to this dataset nor to this domain since instances of fraud are generally much lower than the number of legitimate transactions. After conducting a bit more research into the evaluation of this algorithm I found that using confusion matrices, and even simple ROC curves to gauge model performance are subpar compared to model evaluation using the Area Under the Receiver Operating Characteristic (AUROC).

This evaluation method can handle highly imbalanced data. The higher the AUROC score the better, with a score of one being perfect and .5 being the worst. Additionally, I use the Area Under Recall Curve Precision Score (AUCPR) although that particular metric still isn't as capable as the AUC in dealing with imbalanced data.

Finally, I also make use of the H2O library, which provided strong documentation and an array of algorithms, including Isolation Forests, while also allowing for a simple implementation of the aforementioned evaluation methods.


### Exploring Other Commonly Used Algorithms

While the Isolation Forest algorithm is indeed well suited for finding outliers I wanted to test its performance against a few other algorithms I've worked with, namely logistic regression and another forest-based algorithm, random forest. I chose LR because it is a foundational algorithm that would work well on this particular dataset. In the case of random forest, I wanted to contrast the conceptual/intuitive difference between the Isolation Forest method of selecting random splits to find the leaves that isolate fastest relative to the average depth and isolation to a normal random forests classification model.


For these two algorithms, the data must be treated a little differently. First, I drop the 'Time' column as it is unnecessary in this particular case. Second, I employ a test-train split. Although I'm using the exact same data, I want to maintain a good practice of simply calling the data again into a new pandas data frame.



Results

Islation Forest

Logistic Regression

Random Forest






