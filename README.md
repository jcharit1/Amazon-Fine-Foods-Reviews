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

The motivations are multi-fold: I am curious about the predictive power of macro-level text summary statistics and using word-specific features can make training, storing, and deploying predictive models more computational intensive. 

### Results 

The preliminary analysis shows that a reasonably predictive model can be build without word specific features:  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic.png "AUC ROC on Test Data of Macro-Text Stats Models")  

The best models, k-nearest neighbors, bagged trees, and random forest, achieved AUC ROC of 0.8, 0.8, and 0.81 respectively. While these models are not "highly predictive" (AUC ROC of 0.9+), this is a proof of concept.  

Using a word-specific features approach like bag of words, only resulted in a marginal improvement in predictive performance:  


![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic_BOW.png "AUC ROC on Test Data of BOW Models") 

To be fair, to shorten the training time, I was forced to limit the max_features of the TfidfVectorizer to just 200, I didn't apply any LDA-based dimensionality reduction, and I used fewer models. All three decisions have the potential to significantly limit the performance of word-feature based text classification models. On the other hand, even with these steps to shorten the training time, the models built using macro-level text summary statistics were still trained 10-12x faster. At the very least, this shows that using macro-level text summary statistics as features can be a good option for quickly turning around a solid minimum viable product for prospective clients -- buying your team time to experiment with more time-consuming approaches. 

I also tried combining both approaches. The overall performance improved marginally:  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic_BOW_MERGED.png "AUC ROC on Test Data of BOW + Macro-Text Stats Models")

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
