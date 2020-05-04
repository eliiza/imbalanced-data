# Imbalanced Data
## Introduction

This repository is an auxiliary to my medium blog post on handling imbalanced datasets.  
The data used for this repository is sourced with gratitude from Daniel Perico's Kaggle entry [earthquakes](https://www.kaggle.com/danielpe/earthquakes).

## Installation and requirements

The csv file containing the unmodified data is assumed to reside in the /data folder.

The project requirements can be found in the requirements.txt and installed with

```
pip install -r requirements.txt
```

NB:  
- To run the example of the GAN as a sample generator for oversampling, you have to get the script from the [DIAL](https://www3.nd.edu/~dial/home/ "Data, Inference, Analytics, and Learning")
 team on [github](https://github.com/dialnd/imbalanced-algorithms).
- To run the example using the Hellinger distance criterion in Random Forest, you need to set up the criterion as described in Evgeni Dubov's [github repository](https://github.com/EvgeniDubov/hellinger-distance-criterion).  
(You might also have to copy the resulting files into your notebook directory to import the module) 

## Jupyter Notebooks

The notebooks are standalone examples and can all be executed individually.
To save resources, variables within the notebooks are reused and redefined for each example shown,
hence examples can be executed individually or sequentially as a whole.  
Each model evaluation calculates and prints the precision, recall, ROC, F1 and accuracy scores for the model 
and calculates a confusion matrix, should it be desired.
Unless specified, all notebooks scale the data and one-hot encode the categoricals. This is recommended
practice for many of the resampling algorithms and further ensures that the method is comparable
and compatible with any other algorithms a user might like to try, such as the neural networks in 
notebooks 9 and 10.  

(_NB: as the appendices show, there is a small, reproducible difference in model performance based on
the amount and type of preprocessing_)


### 01 - eda
This notebook contains a short exploration of the dataset. Its main focus is to check the dataset for
an imbalanced class and to examine the missing values and correlations in the dataset.

### 02 - basic, basic_weights, basic_Hellinger
These notebooks have the same structure, they are all using the dataset straight up without any missing
values. The basic notebook contains a default Random Forest and, for good measure, a simple default model 
guessing every event is an earthquake.  
The basic_weights notebook is the same, but the Random Forest uses balanced class weights.
Similarly, the basic_Hellinger notebook uses the Random Forest, but changes the decision criterion to a 
custom model.

### 03 - imputation
This notebook uses the sklearn iterative imputer to 'salvage' more data from the dataset by filling in 
missing data values. The scaling and one-hot encoding are applied before the imputation, with the imputation 
limited to rows that contain fewer than 5 missing values.  

### 04 - knnimputation
This notebook is similar to notebook 03, except it uses the sklearn knnimputer. For the sake of time, 
only rows with fewer than 3 missing values were imputed in this notebook.  

### 05 - undersampling
This notebook uses a standard Random Forest to model a variety of controlled undersampling and cleaning 
undersampling methods.


