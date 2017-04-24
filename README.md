# Amazon Fine Foods Reviews
A series of NLP projects using the Amazon Fine Food Reviews dataset. This repository is a work in progress. I will add projects as inspiration comes. 

## Project One: Predicting Review Helpfulness Using Macro-Level Text Summary Statistics  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic.png "Word Cloud of Helpful Reviews")  

### Summary and Motivation  

The goal of this project is to determine if a review's helpfulness can be predicted without using word specific features (i.e. bag of words and its derivatives). Instead I used macro-level text summary statistics such as:  

1. Number of Sentences
2. Number of Words
3. Readability 
4. Sentiment Metrics

The motivations are multifold: I am curious about the predictive power of macro-level text summary statistics and using word-specific features can make training, storing, and deploying predictive models fairly computational intensive. 

### Results 

The preliminary analysis shows that a reasonably predictive model can be build without word specific features:  

![alt text](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/plots/ROC_Basic.png "AUC ROC on Test Data of Basic Models")  

The best models, k-nearest neighbors and random forest, achieved AUC ROC of 0.8 and 0.81 respectively. While these models are not "highly predictive" (AUC ROC of 0.9+), this is a proof of concept. With more aggressive model training and a more diverse set of macro-level text summary statistics, perhapse the performance of models with word-specific features can be rivaled.

### Key Files

1. [Code to clean the raw data](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/data_cleaning.ipynb)
2. [Code to finalized the raw data](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/finalizing_model_data.ipynb)
3. [Code for training the models](https://github.com/jcharit1/Amazon-Fine-Foods-Reviews/blob/master/code/model_building_part_1.ipynb)



### Strategy

The challenges to building a good model were:  

1. Non-random missing data in the continuous variables.
2. A large number of features given the size of the sample (1,558 vs 3,279).
3. The overwhelming majority of the features are sparse.
4. There is only a moderate number of observations.
5. High class imbalance.

I tackled these challenges by:

1. Turning the continuous variables into binary variables where missing values is a feature.
2. Using algorithms robust to unfavorable feature to observation ratios -- like random forest, which will only use a sample of the features per model fit.
3. Using variance threshold based feature selection and regularized models to avoid introducing too many uninformative sparse features to the model.
4. Avoiding more data-hungry cross-validation strategies like nested cross-validation.
5. Adjusting class weights and using sampling techniques like SMOTE and Tomek Link removal.

### Results

ROC curves on the test data:

![alt text](https://github.com/jcharit1/Identifying-Ad-Images/blob/master/plots/ROC_Best.png "AUC ROC on Test Data of Best Models")

The above strategies resulted in highly predictive models. The best iteration of each model explored had an AUC ROC of 0.95 or greater. The best model was the logistic classifier with a 1:1 class weight, the feature variance threshold set to drop only zero variance features, and no sampling-based class imbalance corrections. 

The performance of the best model was highly stable. The standard deviation of the validation fold AUC ROC was 0.012, a tiny fraction of the average AUC ROC.

The most important features in the dataset seemed reasonable. Listed by order of importance (identified using random forest), the top five are:

1. ancurl*adclick
2. ancurl*adid
3. origurl*misfits2
4. url*static.wired.com
5. ancurl*http+www

Features 1, 2, & 5 seem to be ad attributes and 3 & 4 seem to be the url of the ad source. 

Many more model training approaches were left off of the table. For example, only L2 (ridge) regularization was used for the logistic classifier and SVM were not used (to save time on first iteration of training). However, since the first iteration of training yielded models with an AUC ROC of 0.99, further refinement of the model training process seemed unnecessary.



## Installing

Uses Python 3.5 and anaconda

### Linux 
1. Change into the directory where you want to place the repo
2. Clone it: `git clone https://github.com/jcharit1/Identifying-Ad-Images.git`
3. Change into repo directory: `cd Identifying-Ad-Images/`
4. Edit the environment file prefix (at the end) to reflect your anaconda directory
4. Copy the environment
	1. Option 1, partially copy the environment: `conda env create -f environment_lite.yml`
	2. Option 2, copy the full environment: `conda env create -f environment.yml`
5. Load the environment: `source activate Jimmy_Charite_py35`
6. Make the script executable: `chmod +x ./code/predict_image_type.py`
7. Define the following full file paths via bash variables:
	1. path_old_data (_all of the data used to train and test the models_)
	2. path_colnames (_use the file in the ./raw_data/ subdirectory of the repo_)
	3. path_new_data (_data for new out of sample predictions_)
	4. path_pred_file (_where the predictions will be saved_)
8. Run the prediction script: `./code/predict_image_type.py $path_old_data $path_colnames $path_new_data $path_pred_file`

Copying the full python environment will take 10-15 minutes on a slow internet connection. However, OS specifics aside, it will get you a full mirror of my python environment. Then you should be able run all the notebooks and scripts with, hopefully, no errors. The limited environment (environment_lite.yml) should be sufficient for running the prediction script.

### Windows
TO DO

## Uninstalling

### Linux
1. Delete the repo: `rm -f Identifying-Ad-Images/`
2. Remove the environment: `conda env remove --name Jimmy_Charite_py35`

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

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/jcharit1/Identifying-Ad-Images/blob/master/License.md) file for details
