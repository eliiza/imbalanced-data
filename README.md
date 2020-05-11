# Imbalanced Data
## Introduction

This repository is an auxiliary to my [medium blog post on handling imbalanced datasets](https://medium.com/@eike_germann/modelling-rare-events-c169cb081d8b).  
The data used for this repository is sourced with gratitude from Daniel Perico's Kaggle entry [earthquakes](https://www.kaggle.com/danielpe/earthquakes).
The key idea behind this collection is to provide an even playing field to compare a variety of methods to address imabalance - 
feel free to plug in your own dataset and have a play with the options of the algorithms! It should be fairly easy to change 
what you need or copy and paste to have more comparable elements.  

If you think I've missed an important algorithm or method, please let me know.  

## Results  
This table shows the results of the examples that were run in the notebooks. The best in each category are highlighted
in dark grey, the worst in light grey.
![Alt Text](https://github.com/eliiza/imbalanced-data/blob/master/Interventions.png "Interventions by type")

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

All notebooks are standalone examples and can be executed individually.
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


### 01 - EDA
This notebook contains a short exploration of the dataset. Its main focus is to check the dataset for
an imbalanced class and to examine the missing values and correlations in the dataset.  
For a detailed explanation of the data types in the columns go to the [US Geological Survey description page](https://earthquake.usgs.gov/earthquakes/feed/v1.0/csv.php).  
The exploration is rudimentary since there is no feature engineering besides the creation of a binary class.

### 02 - Basic, Basic_weights, Basic_Hellinger
These notebooks have the same structure, they are all using the dataset straight up without any missing
values. The basic notebook contains a default Random Forest and, for good measure, a simple default model 
guessing every event is an earthquake.  
The basic_weights notebook is the same, but the Random Forest uses balanced class weights.
Similarly, the basic_Hellinger notebook uses the Random Forest, but changes the decision criterion to a 
custom model.

### 03 - Imputation
This notebook uses the sklearn iterative imputer to 'salvage' more data from the dataset by filling in 
missing data values. This notebook does not one-hot encode the data due to resource constraints. The imputation 
is limited to rows that contain fewer than 5 missing values.  

### 04 - Knnimputation
This notebook is similar to notebook 03, except it uses the sklearn knnimputer. For the sake of time, 
only rows with fewer than 3 missing values were imputed in this notebook.  

### 05 - Undersampling
This notebook uses a standard Random Forest to model a variety of controlled undersampling and cleaning 
undersampling methods.

### 06 - Oversampling
This notebook uses a standard Random Forest to model random undersampling as well as a variety of synthetic
oversamplers.

### 07 - Combined sampling
This notebook uses a standard Random Forest to model with two hybrid over- and undersampling methods based
on SMOTE and the EditedNearestNeighbours and TomekLinks algorithms.

### 08 - Balanced ensembles
This notebook uses a variety of modelling algorithms with integrated resamplers instead of the standard Random
Forest while keeping the preprocessing similar to the basic model approach.

### 09 - ANNs
This notebook uses a simple neural network as a point of comparison to the Random Forest model.  
To make the modelling comparable, the training contains a cross-validation step and collects the results to
calculate the average performance of the model.

### 09a - ANNs resampled
For further comparison, this notebook employs the same neural network as notebook 09 and trains the model on data 
preprocessed with SMOTE and data preprocessed with random undersampling.

### 10 - Alternative methods
This notebook applies a different oversampler, a [GAN](https://en.wikipedia.org/wiki/Generative_adversarial_network), 
to generate synthetic data. It's a very interesting approach that, to my surprise, was the only oversampling/synthetic 
method that managed to increase the precision of the default model. This could be an interesting approach for  situations
where precision is the focus, but other tools that successfully increased the precision, like the Hellinger distance
criterion, are out of reach due to platform constraints or security concerns.  
It took a bit of hyperparameter tuning to get
to the result, so a grid search based on the desired metric might be the way to go if the compute is available.  
The most important aspect to note is that the loss functions are related - the generator's loss is dependent on
the discriminator's loss. That makes the loss in itself more unreliable as an indicator of performance, hence training
and evaluation are required to grade the effectiveness of the generator, slowing down the development process. As an example, 
the generator could show a lower loss after 65 epochs of training that after 165 epochs with other parameter settings, 
but the data the former generates performs much worse in training the model than the data from the latter.  
As for which hyperparameters to tune, while the standard deviation of the initialisation had the greatest immediate effect,
the batch size and learning rates appear to be the most significant factors overall. Especially the ratio of learning rates
between the discriminator and generator is important here, since the generator depends on the discriminator to improve.
While the default values have a higher learning rate for the discriminator than the generator, I am not convinced that
that is a general rule to be followed. Depending on the data and the architecture of discriminator and generator, 
it might even be useful to slow down the discriminator with respect to the generator.