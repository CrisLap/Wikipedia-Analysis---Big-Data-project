# Wikipedia-Analysis---Big-Data-project
Wikipedia Analysis - Big Data project with Spark and Databricks

This project consists of 2 different Databricks notebooks:
1. In the first notebook, titled `1.Wikipedia Analysis - EDA`, an Exploratory Data Analysis (EDA) activity has been carried out to analyze and statistically assess the informative content of the dataset. Specifically, using **Spark SQL**, the following quantities have been calculated *for each category*:
     * Number of articles.
     * Average number of words in an article.
     * Number of words in the longest article.
     * Number of words in the shortest article.

    In addition, the most representative word cloud has been identified for each category.
2. The second notebook, `2.Wikipedia Analysis - dataset undersampling & train-test split`,the third one, `3.Wikipedia Analysis - Article Classification_ dataset with summary column` and the last one `4.Wikipedia Analysis - Article Classification_ dataset with documents column` focus on training a text classifier capable of categorizing Wikipedia articles into 15 different categories. The dataset consists of 4 columns:
    * `title`: article's title.
    * `summary`: article's summary.
    * `documents`: complete article.
    * `category`: category associated to the article.

    The idea has been that of performing a hyperparameter tuning procedure to find the best model using the `summary` column, and then training the same model using the `documents` column, to see if there were significant improvements in using the entire articles instead of their summaries. Before arriving at doing this, different operations have been executed:
    * Data cleaning (removal of missing and duplicate values).
    * Undersampling of all classes except the `politics` class, choosing $0.33$ as the value of the ratio of the number of samples in the minority class (i.e., `politics`) over the number of samples in the majority class after resampling. This was done instead of assigning different weights to the various categories, which significantly increased the training time of the classifier, to the extent that its usage had to be relinquished.
    * Stratified train/test/validation split. This was done instead of using 3-fold cross validation, for computational speed reasons related to the limits imposed by the cluster offered by Databricks Community Edition.
    * Text preprocessing, during which several steps have been undertaken:
        - Conversion of the `category` column into numeric indices.
        - Text cleaning, consisting of a first part where tokenization of text, lowercase conversion, and removal of punctuation, digits, and whitespace have been carried out. Then, common English stop words have been removed, and lemmatization has been applied. Finally, only cleaned documents with a minimum token count have been retained.

    Once doing this, the hyperparameter tuning has been performed. In particular, the pipeline being tuned was made of:
    * a text feature extraction part, consisting of a TF-IDF (Term Frequency-Inverse Document Frequency) transformation, followed by a normalization of the resulting values.
    * the model to train. For computational speed reasons, it has been decided to run the algorithm only considering Naive Bayes as the classifier type

    After the hyperparameter tuning, the model's hyperparameters providing the highest accuracy have been used and its performance on the test set has been assessed, by computing the accuracy value and building the confusion matrices for both the train and test set. Finally, the same model has been trained using the `documents` column, in order to compare the performance of the two models and see what was the most accurate one.

To correctly visualize the notebooks and discover more details about the project, you can import the files in Databricks `.dbc` archive format into your Databricks workspace using the procedure outlined [here](https://docs.databricks.com/en/notebooks/notebook-export-import.html).
