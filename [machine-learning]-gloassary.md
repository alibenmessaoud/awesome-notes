- “Artificial intelligence is a technology or computer system designed to function in a way that simulates how the human brain thinks.
- Machine learning is a subset of AI which involves ‘training’ machines to ‘learn’ from sets of data, enabling them to draw insights and make predictive decisions. It automates tasks and finds patterns or anomalies, learns from them and creates new rules for next time.
- Deep learning is currently the most advanced subset of machine learning, and thereby a subset of AI, which intends to bring machines as close as possible to human levels of thinking. According to MIT Technology Review, “The software learns, in a very real sense, to recognize patterns in digital representations of sounds, images, and other data” by creating an artificial neural network.
- Natural Language Processing (NLP) is an element of deep learning that involves translating text or human ways of speaking so that a computer is able to categorize and make sense of it.”





Machine learning explained
Machine learning algorithms are often divided into supervised (the training data are tagged with the associated answers) and unsupervised (labels that may exist are not shown to the training algorithm). 

Supervised machine learning problems are further divided into classification (predicting non-numeric answers) and regression (predicting numeric answers).

Unsupervised learning is further divided into clustering (finding groups of similar objects), association (finding common sequences of objects), and dimensionality reduction (projection, feature selection, and feature extraction).

Classification algorithms
A classification problem, under the supervised learning, asks for a choice between two or more classes, usually providing probabilities for each class. The most common and basic algorithms are Naive Bayes, Decision Tree, Logistic Regression, K-Nearest Neighbors, and Support Vector Machine (SVM). You can also use ensemble methods (combinations of models), such as Random Forest, other Bagging methods, and boosting methods such as AdaBoost and XGBoost.

Regression algorithms
A regression problem, under the supervised learning,  asks the model to predict a number. The simplest and fastest algorithm is linear regression, but you shouldn’t stop there, because it often gives you a mediocre result. Other common machine learning regression algorithms include Naive Bayes, Decision Tree, K-Nearest Neighbors, Learning Vector Quantization, LARS Lasso, Elastic Net, Random Forest, AdaBoost, and XGBoost. You’ll notice that there is some overlap between machine learning algorithms for regression and classification.

Clustering algorithms
A clustering problem, under the unsupervised learning, asks the model to find groups of similar data points. The most popular algorithm is K-Means Clustering; others include Mean-Shift Clustering, DBSCAN (Density-Based Spatial Clustering of Applications with Noise), GMM (Gaussian Mixture Models), and HAC (Hierarchical Agglomerative Clustering).

Dimensionality reduction algorithms
Dimensionality reduction, under the unsupervised learning, asks the model to drop or combine variables that have little or no effect on the result. This is often used in combination with classification or regression. Dimensionality reduction algorithms include removing variables with many missing values, removing variables with low variance, Decision Tree, Random Forest, removing or combining variables with high correlation, Backward Feature Elimination, Forward Feature Selection, Factor Analysis, and PCA (Principal Component Analysis).

Optimization methods
Training and evaluation turn supervised learning algorithms into models by optimizing their parameter weights to find the set of values that best matches the ground truth of your data. The algorithms often rely on variants of steepest descent for their optimizers, for example stochastic gradient descent, which is essentially steepest descent performed multiple times from randomized starting points. Common refinements on stochastic gradient descent add factors that correct the direction of the gradient based on momentum, or adjust the learning rate based on progress from one pass through the data (called an epoch or a batch) to the next.

Data cleaning for machine learning
There is no such thing as clean data in the wild. To be useful for machine learning, data must be aggressively filtered. For example, you’ll want to:

1. Look at the data and exclude any columns that have a lot of missing data.
2. Look at the data again and pick the columns you want to use (feature selection) for your prediction. This is something you may want to vary when you iterate.
3. Exclude any rows that still have missing data in the remaining columns.
4. Correct obvious typos and merge equivalent answers. For example, U.S., US, USA, and America should be merged into a single category.
5. Exclude rows that have data that is out of range. For example, if you’re analyzing taxi trips within New York City, you’ll want to filter out rows with pickup or drop-off latitudes and longitudes that are outside the bounding box of the metropolitan area.

There is a lot more you can do, but it will depend on the data collected. This can be tedious, but if you set up a data cleaning step in your machine learning pipeline you can modify and repeat it at will.

Data encoding and normalization for machine learning
To use categorical data for machine classification, you need to encode the text labels into another form. There are two common encodings.

One is label encoding, which means that each text label value is replaced with a number. The other is one-hot encoding, which means that each text label value is turned into a column with a binary value (1 or 0). Most machine learning frameworks have functions that do the conversion for you. In general, one-hot encoding is preferred, as label encoding can sometimes confuse the machine learning algorithm into thinking that the encoded column is supposed to be an ordered list.

To use numeric data for machine regression, you usually need to normalize the data. Otherwise, the numbers with larger ranges might tend to dominate the Euclidian distance between feature vectors, their effects could be magnified at the expense of the other fields, and the steepest descent optimization might have difficulty converging. There are a number of ways to normalize and standardize data for machine learning, including min-max normalization, mean normalization, standardization, and scaling to unit length. This process is often called feature scaling.

Feature engineering for machine learning
A feature is an individual measurable property or characteristic of a phenomenon being observed. The concept of a “feature” is related to that of an explanatory variable, which is used in statistical techniques such as linear regression. Feature vectors combine all the features for a single row into a numerical vector.

Part of the art of choosing features is to pick a minimum set of independent variables that explain the problem. If two variables are highly correlated, either they need to be combined into a single feature, or one should be dropped. Sometimes people perform principal component analysis to convert correlated variables into a set of linearly uncorrelated variables.

Some of the transformations that people use to construct new features or reduce the dimensionality of feature vectors are simple. For example, subtract Year of Birth from Year of Death and you construct Age at Death, which is a prime independent variable for lifetime and mortality analysis. In other cases, feature construction may not be so obvious.

Splitting data for machine learning
The usual practice for supervised machine learning is to split the data set into subsets for training, validation, and test. One way of working is to assign 80% of the data to the training data set, and 10% each to the validation and test data sets. (The exact split is a matter of preference.) The bulk of the training is done against the training data set, and prediction is done against the validation data set at the end of every epoch.

The errors in the validation data set can be used to identify stopping criteria, or to drive hyperparameter tuning. Most importantly, the errors in the validation data set can help you find out whether the model has overfit the training data.

Prediction against the test data set is typically done on the final model. If the test data set was never used for training, it is sometimes called the holdout data set.

There are several other schemes for splitting the data. One common technique, cross-validation, involves repeatedly splitting the full data set into a training data set and a validation data set. At the end of each epoch, the data is shuffled and split again.

Machine learning libraries
In Python, Spark MLlib and Scikit-learn are excellent choices for machine learning libraries. In R, some machine learning package options are CARAT, randomForest, e1071, and KernLab. In Java, good choices include Java-ML, RapidMiner, and Weka.

Notes from https://www.infoworld.com/article/3512245/deep-learning-vs-machine-learning-understand-the-differences.html 