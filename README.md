
Adversarial Validation
========================

# What is Adversarial Validation?
The objective of any predictive modelling project is to create a model using the training data, and afterwards apply this model to the test data. However, for the best results it is essential that the training data is a representative sample of the data we intend to use it on (*i.e.* the test data), otherwise our model will, at best, under-perform, or at worst, be completely useless.   

***Adversarial Validation*** is a very clever and very simple way to let us know if our test data and our training data are similar; we combine our `train` and `test` data, labeling them with say a `0` for the training data and a `1` for the test data, mix them up, then see if we are able to correctly re-identify them using a binary classifier.

If we cannot correctly classify them, *i.e.* we obtain an area under the [receiver operating characteristic curve](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) (ROC) of 0.5 then they are indistinguishable and we are good to go.

However, if we can classify them (ROC > 0.5) then we have a problem, either with the whole dataset or more likely with some features in particular, which are probably from  different distributions in the test and train datasets.
If we have a problem, we can look at the feature that was most out of place. The problem may be that there were values that were only seen in, say, training data, but not in the test data. If the contribution to the ROC is very high from one feature, it may well be a good idea to remove that feature from the model.


## Adversarial Validation to reduce overfitting
The key to avoid overfitting is to create a situation where the local cross-vlidation (CV) score is representative of the competition score. When we have a ROC of 0.5 then your local data is representative of the test data, thus your local CV score should now be representative of the Public LB score.

Procedure:

* drop the training data target column 
* label the `test` and `train` data with `0` and `1` (it doesn't really matter which is which)
* combine the training and test data into one big dataset
* perform the binary classification, for example using XGboost
* look at our AUC ROC score

## Install
adval can be installed using
```
pip install adval
    
