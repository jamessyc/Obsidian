## What is Machine Learning?

A set of methods that can automatically detect patterns in data, and then use the patterns to predict future data or perform other kinds of decision making.

Gives computers the ability to learn without being explicitly programmed.

## Supervised learning

Function approximator

- Given x, given y, trying to find the best way to get from x to y (f)
    

3 components

- Representative training data
    

- Dataset annotated with labels
    

- Feature extraction methods
    

- Features = observable properties of the data based on which the model will try to make predictions from
    

- Machine learning algorithm
    

- Start with a training set that contains the right answers yi:
    

- D = {(xi, yi)}Ni=1
    
- Set of input features xi, and outputs yi.
    
- Supervised learning algorithm studies the training set and develops a function that maps features x to a class y: f(x) = y
    

## Generalisation

- We want an ML model that performs well on new unseen data
    

- In other words, we want our model to generalise well
    
- This is important because of the ‘long tail’
    
- A.k.a. the training set cannot encapsulate all possible cases, there are always some cases that will be encountered that haven’t been seen previously
    

## Classification vs Regression

Based on the variable y we are trying to predict, the problem is either classification or regression

- If y is categorical from some finite set, then it is a classification problem
    
- If y is a real valued scalar, then it is a regression problem
    

### Classification

Let C be the number of classes

If C == 2, then we have a binary classification problem

If C > 2, then we have a multiclass classification problem

  

We can think of it as function approximation

if y = f(x), then learning is trying to estimate f, creating an estimate f’.

#### K-Nearest Neighbours

How does kNN work?

K stands for how many K neighbours we care about

For example, if K = 5, then for a given point, we only care about the 5 nearest neighbours. Imagine neighbours as a sorted list, sorted by the euclidean distance to the point we care about. K=1 means we take the head of the list (closest element), K=2 means we take the first two elements (two closest elements), etc.

Potential issues

With an even number of K, you may end up with a 50/50, meaning a point has an equal probability of being in more than one class

With an odd number of K, you get rid of that, but you may also get more than K points with the same distance. E.g., if you have K=3, but you get 4 nearest points with all the same difference, then your classification is based on which of these you choose. (many ways to get over this, for example can increase K)

#### Probabilistic prediction

For ambiguous cases, we can return a probability

p(yi | xi, D) = The probability of being in each class, given the input and the set of training data

- Probability classifiers return a distribution over classes
    

- E.g. if we have 5 classes and an input, we might get probability 0.1 for class 1, 0.2 for class 2, etc.
    

We can also represent the probability like this:  
p(yi | xi, D, M) where M is the model we are using to make the prediction

  

Given a probability distribution, we can decide which class to assign the input by choosing the class with the highest probability, or the mode.

  

This can be represented using the arg max function:

y’ = arg maxCc=1 p(y = c | xi, D)

We want to find the class c out of all classes C that maximises the probability on the right hand side

(Covered in more detail later on)

### Regression

#### Linear Regression

Assumes that the output y is a linear combination of the input x.

In other words:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeJL8TBi6QKawox8eHacywZGtx1UldlCtDdw0YhTvnH3PDnnliGpGv2l0085eqzfTvdezZZB9TPdnGAIthOpzNql3A2Y7PoU-aCLvbjLIzDyfjAQU2-xp2sF402EcAxHTIf_UqlwQ?key=DAssY80HPaH4Gt4ccOL2V3vW)

The output y is calculated as a weighted linear combination of the input x.

For j inputs, we have j weight vectors.

Epsilon is the residual error.

  

For a one dimensional input x, we write:

x = (x0, x1)

x0 = 1

x1 = x

Which is the same as saying x = (1, x). Then:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe-KSHiNPSfz6zsr6iN_JM4DaxhknhRaxeZYsBBBWFW85H9tKAnttPBpjIIKLumqcY63rOgezGMd08ceEh51KcGqX5lZ3XLBH-FQK-D8XcCdJUeP5J1Zczzr18Jr27XSeO25RVpNQ?key=DAssY80HPaH4Gt4ccOL2V3vW)

Where w0 is the intercept and w1 is the slope.

## Unsupervised Learning

Pattern recognition

- Given Dataset d containing points x, trying to find some trend / pattern in D
    

  

No labels or annotated data to learn from

Start from a set of examples and look for patterns / interesting structures in the data

- The most common form of unsupervised learning is clustering.
    
- Data within a cluster should be similar
    

We can think of unsupervised learning as density estimation

Where we take the task as building models of the form:  
p(xi | D)

Or the probability of the features, given the data

- Differences from supervised learning:
    

- We no longer have a set of class labels to predict
    
- xi is a vector of features, so our prediction is a multivariate probability distribution
    

- As opposed to a univariate one (where we had y as a single variable that we were trying to predict)
    

### Clustering

Need to estimate how many clusters there are

Need to find K (number of clusters) such that p(K | D) is maximised:

K* = arg maxK p(K | D)

This is an example of model selection - choosing the right parameters for our model

  

## Basic concepts

### Parametric vs. Non-parametric

Does the technique make assumptions about the structure of the data?

- E.g. ‘there are four Gaussian clusters’
    
- If so, it is parametric
    
- If not, it is non-parametric
    

#### Pros and Cons

- Parametric models can be computationally simpler
    
- But their assumptions about the dataset can lead to inaccuracy
    
- Non-parametric models are more flexible, but can be computationally inefficient for large datasets
    

### Scaleability

- E.g. kNN
    

  

- kNN classifiers are simple and can work well, but scale badly with higher dimensional inputs
    

- i.e. we need to calculate distances - in a dataset with a high number of dimensions, this can become costly
    

### Curse of dimensionality

To better distinguish between examples, we can add more features (dimensions)

But more features needs more examples, otherwise we overfit

In general, the relationship between the two is exponential, i.e. n more features will need x^n more examples

  

We can address this using parametric models, to make an assumption about the best way to distinguish between examples

### Overfitting

When we relax constraints, models can become too specialised to the data

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXflVLH32kSXf3AfkxukJ71JMlvaVDftXKNrK5HV9N8iQmZTHEuxcTkLHPPUcCfYpgVM2qgKK1SNSriiOWo7pcwOo54iYCwAYuZUKhCDRvBUc0qA3igFaGDQuM3TQQSdfkvYRQqZ5g?key=DAssY80HPaH4Gt4ccOL2V3vW)

How can we tell when we have overfit? -> Performance metrics

## Performance measurement

How do we know if we have learnt well?

- In supervised learning, this means ‘can we predict y well given x?’
    
- Compute the misclassification rate:
    

- ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeCBGxqkumdLbvPI2fPorccG6QcodbqyMirU0D-4FZvGCAju04F9meziPZZ96aMAT9wy5BUHvgTv00wtNGUZIB0kXUschJpsoMXBWYoahsGPPwrgxRYywVKWoDAzfgFTw16GEKNjA?key=DAssY80HPaH4Gt4ccOL2V3vW)
    

- Or, conversely, the classification rate (correctness)
    

### Unseen test set

Misclassification rate gives us some idea whether we have learned well from the training data, but not how well it generalises

To test this, we can use an unseen test set, label it, and calculate the misclassification rate

### Learning curve

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcPSj0jKFW-ih9U-yb78U1m5LCGxrB9QLdNf2X2fnH4-KSIWDLYaEl_Ft4SbfUIWxfC8WjqguTaBEpVOz2kyevhmoeS6nyVj68PzHvJRnExsClr_NGxN2h0nwIwFUIO9aRXP8Z67g?key=DAssY80HPaH4Gt4ccOL2V3vW)

Learning curve = % correct on test set as a function of training set size

  

But if we tune the parameters of our model only by looking at performance on the test set, we are again overfitting.

### Set partitions

Split training set into three subsets

- Training set (80%)
    

- For training the model
    

- Validation / development set (10%)
    

- For adjusting parameters and selecting model
    

- Test set (10%)
    

- For evaluating the generalisation performance of the model produced by the validation set
    

Want enough data to tune parameters, but also enough data to properly test generalisation performance

  

What if the training set is too small?

#### K-fold cross-validation

Terminology: Fold = subset

Split data into k equal subsets to test on. Train on k-1 sets and test on the remainder subset only.

  

That is, for each row in the diagram below, you are training and testing on different subsets of the data.

  

Repeat k times, and calculate the misclassification error average over all k test subsets.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeQbrCgHkrth4Mv9VpmifAMg0aM3WIoRn2tXBbJp-Boxh0Lm5Cofs1anAMXl_H350eGicLrTKXtyQ5iyjsDczmJlIuRxwWlR6nx-b86JQtnFu_yVxnm2UCASZo0yT-WQSL2hu7QtQ?key=DAssY80HPaH4Gt4ccOL2V3vW)

The final performance is the average of the performances of each subset. (Average test set score is a better estimate of the error rate than a single score).

  

Common values of k are 5 and 10 (5-fold or 10-fold)

The extreme case is where k = the number of data points, which is called leave-one-out cross validation (because in each iteration we train on every single example except one, which we use for testing)

### Model selection and/or parameter tuning

Can use cross-validation for validation and training instead, and maintain test set as a separate unseen set

### No free lunch theorem

There is no best model for all scenarios, so we have to do model selection. Different models also have different tradeoffs, between speed, accuracy and complexity.

## Evaluation metrics

### Accuracy

Accuracy = correct / total

  

For a binary classification problem, an accuracy of >50% is ‘doing something’

  

But what if false negatives are more important than false positives?

- E.g. in breast cancer, where we really don’t want to miss a case 
    

### Confusion matrix

  

|   |   |   |   |
|---|---|---|---|
|||actual|   |
|||yes|no|
|predicted|yes|true positive|false positive|
|no|false negative|true negative|

### Precision

Precision = True positives / (True positives + False positives)

How many of the ‘yes’ predictions we made are correct?

Horizontal row

### Recall

Recall = True positives / (True positives + False negatives)

How many of the ‘yes’ examples in the data did we predict correctly?

Sensitivity, or true positive rate.

Vertical column

  

As we increase precision, recall decreases.

### F1 score

Balance of the precision and recall, weighing them equally

F = 2(precision * recallprecision + recall)

#### FB score

Allows a weighting B of precision and recall:

FB = (1+B2)(precision * recallB2precision + recall)

Examples include F2 and F0.5

where F2 weights recall higher, and F0.5 weights precision higher.Probability metrics

Bigger number = recall weighted higher

Smaller number = precision weighted higher

Above, we assume deterministic results

But generally classification problems are probabilistic.

Thus, can turn a probability into a definite ‘yes’ or ‘no’ using a threshold, where:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcK_qczAftudYPPVHY8R62VRAj4qUOcR-EN8Q0WZkpFShi23a75DSGPdD674JTQn1dNRyF1U5d0nnU-3AAcMxLKdoFs4LjlkyebIvt9LgZ-JpuLRbRHCPFFjvSRP88JDRSQWg2Dew?key=DAssY80HPaH4Gt4ccOL2V3vW)

Changing the threshold changes TP and FP.

Instead of picking a threshold, we can analyse the ROC curve.

### ROC Curve

Plot the true positive rate against the false positive rate

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeDGDTJAmEsQFwv4z7psf4N2s9X8ZWiuwJ4KzrltWz1rFFVLg6ghqlyBHaYkqPT-fgZ77zrzbRUxxLrKUwj3U3hwuaAFky5aalKxNe-SxkmNC9Pf2lCWPLgXFl86RacRvk7wl_rHg?key=DAssY80HPaH4Gt4ccOL2V3vW)

The closer the blue line is to the top left, the better our model is.

### Evaluating clusters

If you know what the clusters should be, then the classification measures can be used.

But most of time we don’t

#### External evaluation

E.g. how well the number of clusters serves a downstream task

#### Internal evaluation

Try to establish how coherent the clusters are and how well they are separated from each other

[[2 - Inductive Learning]]