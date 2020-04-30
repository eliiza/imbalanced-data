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

## Jupyter Notebooks

The notebooks are standalone examples and can all be executed individually.
To save resources, variables within the notebooks are reused and redefined for each example shown,
hence examples can be executed individually or sequentially as a whole.  



