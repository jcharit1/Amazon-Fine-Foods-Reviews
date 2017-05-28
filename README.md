# Amazon Fine Foods Reviews
A series of NLP projects using the Amazon Fine Food Reviews dataset. This repository is a work in progress. I will add projects as inspiration comes. 

## Project One: Predicting Review Helpfulness Using Macro-Level Text Summary Statistics  

_Word Cloud of Helpful Reviews_  
![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/helpful_reviews_word_cloud.png "Word Cloud of Helpful Reviews")  

### Summary and Motivation  

The goal of this project is to determine if a review's helpfulness can be predicted without using word specific features (i.e. bag of words and its derivatives). Instead I used macro-level text summary statistics such as:  

1. Number of Sentences
2. Number of Words
3. Readability 
4. Sentiment Metrics

The motivations are multi-fold: I am curious about the predictive power of macro-level text summary statistics and using word-specific features can make training, storing, and deploying predictive models more computationally intensive. 

Stepping back, the whole excercise of predicting review helpfulness is worthwhile because it can improve customer statisfaction. A major annoyance from online shopping is the product will not be as satistisfying as it is advertised. Many shoppers depend on helpful reviews to avoid buyer's remorse. Therefore placing helpful reviews front and center for as many products as possible can improve the shopping experience, customer retention, and eventually profits. 

Traditionally online businesses solve this problem by allowing customers to rate the product reviews of other customers, then rank the reviews by their ratings. This can be insufficient for very popular products where potentially highly helpful reviews are never rated by customers because they are drowned out by unhelpful or moderately helpful reviews. Also, for new or niche products, some potentially highly useful reviews can go unrate because customer volume to the product page is too low. Predictive models can step in to identify potentially highly helpful reviews and help the web design team ensure that customers can easily find them.  

Now given the potential business benefit of a review helpfulness model, why not spend as much resources as needed to train, store, and deploy one? Every resource spent on one project is a resource not spent on another project, therefore it is important to be aware of options for intelligently trading off the resources invested in a project with the terminal quality of that project.  

### Results 

The preliminary analysis shows that a reasonably predictive model can be built without word specific features:  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic.png "AUC ROC on Test Data of Macro-Text Stats Models")  

The Area under the curve of the ROC (AUC ROC) of the best models, k-nearest neighbors, bagged trees, and random forest, were 0.8, 0.8, and 0.81 respectively. While these models are not "highly predictive" (AUC ROC of 0.9+), this is a proof of concept.  

Using a word-specific features approach like bag of words only resulted in a moderate improvement in predictive performance:  


![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic_BOW.png "AUC ROC on Test Data of BOW Models") 

To be fair, since I was training on a personal computer, I was forced to make decisions that potentially significantly limited the performance of the bag of words approach, but kept the training time under 48 hours.  

For example, I limited the number of words that can be used as features to 200 (setting max_features of the TfidfVectorizer estimator to 200). It is possible that the words that are most predictive of helpfulness appear infrequently enough that they will drop out under the 200 word feature limit. The extent of this issue was partially attenuated by the text preprocessing I did when preparing the reviews for the model. I used the [lemma](https://en.wikipedia.org/wiki/Lemmatisation) of the words to collapse the many forms that they can take ([their inflected forms](https://en.wikipedia.org/wiki/Inflection)) into one base form [their lexeme](https://en.wikipedia.org/wiki/Lexeme). This essentially does a preliminary dimensionality reduction of the data, reducing the number of highly infrequent words and, as a result, the number of words that will be dropped out by the word feature limit.  

Also, I didn't apply any Latent Dirichlet Allocation (LDA) analysis to the word features (or rather I gave up as the training time prolonged beyond what was practical). LDA is a powerful technique for discovering topics included in documents. It is possible that LDA could have identified the key topics that make food reviews very helpful, like taste or appearance of the food for example, and therefore boosted the performance of the models.  

On the other hand, however, even with these steps to shorten the training time, the models built using macro-level text summary statistics were still trained 10-12x faster. At the very least, this shows that using macro-level text summary statistics as features can be a good option for quickly turning around a solid minimum viable product -- buying the data science team time to experiment with more time-consuming approaches. Moreover, while a data science team will have access to much more powerfull computers, they may need to train over a much larger dataset to ensure that the model is reliable for a wide variety of product reviews. Therefore, even for highly resourced teams, options for intelligently trading off model quality and the time it takes to train and deploy them are still useful.

Finally, I tried combining both approaches. The plot below shows the bootstrapped AUC ROC of the random forest model for when the macro-text stats and/or bag of words features were used:  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/BoxPlot_ROC_MacText_BOX.png "Comparison of AUC ROC on Test Data of BOW + Macro-Text Stats Models") 

Interestingly, though not entirely surprising, combining both types of features improved the model across the board. Both the minimum, maximum, and mean AUC ROC improved.

### Strategy and Tools

For its combination of speed and parsimony, I used [spacy](https://spacy.io/) to process the text and count the number of words/sentences. The package [textstat](https://pypi.python.org/pypi/textstat/0.1.6) was used for the readability score (Automated Readability Index) and the [NLTK](http://www.nltk.org/install.html) package was used for sentiment analysis. All the machine learning was done with scikit-learn.

### Next Steps

Next I want to improve the model performance by experimenting with:

1. Different measures of text readability
2. Using sentence, as oppose to whole-review level, measures of sentiment
3. Use vector representations of words to capture semantic summaries of reviews
4. Formally addressing the moderate class imbalance problem using SMOTE, over/under sampling, and Tomek Link removal
5. Use topic modeling to identify topics that are highly predictive of review usefulness

### Key Files

1. [Code to clean the raw data](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/data_cleaning.ipynb)
2. [Code to finalized the raw data](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/finalizing_model_data.ipynb)
3. [Code for training the basic macro-text stats models](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/model_building_part_1.ipynb)
4. [Code for training the bag of words models](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/model_building_part_2.ipynb)
5. [Code for training the combined bag of words and macro-text stats models](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/model_building_part_3.ipynb)

## Installing

Uses Python 3.5 and anaconda

### Linux 
1. Change into the directory where you want to place the repo
2. Clone it: `git clone https://github.com/jcharit1/Amazon-Fine-Foods-Reviews.git`
3. Change into repo directory: `cd Amazon-Fine-Foods-Reviews/`
4. Edit the environment file prefix (at the end) to reflect your anaconda directory
4. Copy the environment `conda env create -f environment.yml`
5. Load the environment: `source activate py35`
6. Open the notebooks: `jupyter notebook`

Copying the full python environment will take 10-15 minutes on a slow internet connection. However, OS specifics aside, it will get you a full mirror of my python environment. Then you should be able run all the notebooks and scripts with, hopefully, no errors.

I didn't include the raw data to avoid slowing down git commits. It can be found here: [link to raw data](https://www.kaggle.com/snap/amazon-fine-food-reviews)

### Windows
TO DO

## Uninstalling

### Linux
1. Delete the repo: `rm -f Amazon-Fine-Foods-Reviews/`
2. Remove the environment: `conda env remove --name py35`

### Windows
TO DO

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Author

[Jimmy Charité](https://github.com/jcharit1)
jimmy.charite@gmail.com

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/License.md) file for details
