## Fraud Detection With Isolation Forest And Other Common Classifiers

In this project, I explore the implementation and evaluation of several algorithms used for fraud detection. My first and primary model will be created using the isolation forest algorithm which makes use of simple intuition. Used as an ensemble method, it randomly selects a feature to serve as the root of each decision tree and further splits the data based on random values taken from those features. By doing this a norm can be ascertained by finding the average depth required to isolate each point of data and computing an average depth. And since anonymous data does not trend with normal data, these transactions can be detected because their depths are outside of that norm.


### Data

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/head.jpg "Anonymized Features")

The data for this project is from [Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud) and contains 170885 rows of anonymized data represented by 28 features anonymized as floating-point values along with 'Time', 'Amount', and 'Class'. During EDA I won't find much of interest in 'Time' so that will be dropped, 'Amount' is simply the dollar amount associated with each transaction, and 'Class' is divided into the 360 labelled 1 for fraud, and the rest labelled as 0 for normal.

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/freq.jpg "Scales Match")

Nothing significant in time so I'll drop it and focus on dollar amounts..

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/histo.jpg "Normal Transactions Are 10x")

'Amount' appears to be a strong indicator for fraud since the entire range of fraudulent transactions is almost perfectly 10 times smaller than normal transactions. Seeing as this is highly unlikely, I surmise that the dataset is either manufactured or trimmed to these dimensions. In any case, fraudulent transactions are generally much smaller than legitimate ones.


![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/cor.jpg "Correlations to Fraud")
Examining the correlation matrix with regard to data labelled as class 1 we can see that amount carries a bit more variance and some of the strongest correlations as well.




![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/imbalance.jpg "Only 360, or 0.21% are labelled Fraud")

The data is also extremely imbalanced, with only 360 of 170k, or 0.21%, of transactions being labeled as fraud. This means I'll not be able to rely on accuracy scores to evaluate my models, instead, I'll monitor recall and precision. Also, there are no fraud loss/handling costs provided so finding a tradeoff won't need to be considered here.



![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/2D.jpg "")
Principal component analysis also illustrates outliers.


### Models

I chose to start my analysis with the [Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html) algorithm which is especially good at finding outliers. As an ensemble method, it randomly selects a feature for each tree's root and also selects a random value along with the range of those features values to split upon. The intuition follows that an outlier to the data will require a much shorter path to its isolation, i.e. a pure leaf. The difference between the length of this path and the average depth required by normal data is then computed as the score. To compare and explore evaluations I also created linear regression and random forest models, which performed much better than isolation forest. Something very curious about this algorithm is that it reads in the h2o data frame format instead of a pandas DataFrame.



### Findings
I suspect that a major reason why the random forest and regression models outperformed the isolation forest is because of that h2o data frame format. A way to test this would be to use that library's implementations and data type.

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/Iso.jpg "Isolation Forest")

Isolation Forest

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/RF.jpg "Random Forest")

Random Forest

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/LR.jpg "Logistic Regression")

Logistic Regression


Still, this was a great opportunity to use ROC and AUROC as evaluation metrics.

![alt text](https://github.com/333Kenji/Fraud-Detection/blob/main/images/ROCs.jpg "ROC and AUROC for Isolation Forest")

In particular, AUROC can handle highly imbalanced data, and the higher the AUROC score the better, with a score of one being perfect and .5 being the worst. Additionally, I use the Area Under Recall Curve Precision Score (AUCPR) although that particular metric still isn't as capable as the AUC in dealing with imbalanced data.
