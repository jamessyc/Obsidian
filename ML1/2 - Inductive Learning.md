Inductive learning: learning from examples (supervised learning)

Inductive learning hypothesis: Any hypothesis found to approximate the target function well over a sufficiently large set of training examples will also approximate the target function well over other unobserved examples.

## Decision Trees

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcno_iDflvqhgs33VDbL5v_wEhYrIyx7dwITW1k6FBPkvhdkOR-wEyb3RIU-Ihm6310D8RRDl2WztGJbVuWzKaOFjPviwVLWYtRNf9LZ7fWBm25NWU_F6-K6-vaBLZiXLYIgqKIWw?key=DAssY80HPaH4Gt4ccOL2V3vW)

Each node is a test on the value of a feature for a specific data point.

On each node, we move left or right based on the value, until we reach a leaf node.

Each path to a leaf is a conjunction of feature tests, and the tree itself is a disjunction of these conjunctions.

### Example

Should we wait for a table at a restaurant?

#### Possible features:

- Is there an alternative nearby?
    
- Does the restaurant have a bar?
    
- Is it Friday?
    
- Are you hungry?
    
- How full is the restaurant?
    
- What’s the price range?
    
- etc.
    

#### Dataset

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeDpfq8Esze9p2v6RRrdCUkletyy0CkyRhOcXgjk1w0d6tw_h2lzRgO7rF53dGWkSX6Yf55DlAEzRquYIxEQZzSXXWtOoL1gJCGUZz6yOneVOWRCI4dbam7TL56cMozgI6PYU3S-Q?key=DAssY80HPaH4Gt4ccOL2V3vW)

#### Model

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeoqLGkiYu792O0SkOn7vZDRVh6RF4P568jef7Gd_8Yw8igFf4u4mN9Vb9jYAG_rnle5vyci9ldj-e5WLq4M052E7cSws23UgIPiAk_0gyH00UfXid-YdEno6DfM8tJ4V_5pzJDdQ?key=DAssY80HPaH4Gt4ccOL2V3vW)Decision Tree algorithm takes the dataset as input, and uses it to learn a tree structure that fits the data and can be used for predictions.

### Expressiveness and optimisation

Simplest solution would be to have one path to each leaf node for each example.

- Probably doesn’t generalise well. We want more compact decision trees.
    

So, our goal is to find a small tree that is consistent with the training examples.

- To do this, we choose the most important feature first. We can do this recursively to form the smallest tree possible.
    

#### Choosing the best features

Features are ‘better’ if they divide the examples into groups of similar classes. For example:![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeNVX_mizOU8wIkGWBbhWn3bIulTURMyAQZYEyQHZdxaZGWHdzSMwKXoWIk01kvYF3iG0NBKU8b3IFYdf59mXIaB7Td-wT08GOTudrWzfpi4j_qFnmHrRAixpf0Ox3i9SMLTs0Z?key=DAssY80HPaH4Gt4ccOL2V3vW)

On the left, the Type of restaurant is not a good feature to choose because the groups it forms have a fairly equal number of ‘Yes’s and ‘No’s (represented by the shades of the squares). However, Patrons is a better feature to choose, as the resulting groups have distinct ‘Yes’ and ‘No’s, for ‘Some’’ and ‘None’ respectively.

In general, one of five things can happen when we choose a feature:

1. If all remaining examples are positive, then we are done, and we can answer ‘Yes’ (e.g. in the ‘Some’ case for the Patrons feature)
    
2. Similarly, if all remaining examples are negative, we can answer ‘No’. (See ‘None’ in Patrons)
    
3. If there are some positive and some negative, then we choose another feature (the next best one) to split them.
    
4. If there are no examples left, then there are no examples that fit this case, meaning that for the combination of features the path represents, we have no data.
    

5. This means we have to return some default value.
    
6. Plurality classification: Best guess is based on the parent node. Methods:
    

7. Majority-based: If parent node classifies most examples as ‘Yes’, then return ‘Yes’ (or vice versa for No)
    
8. Random pick: Weighted by ratio of examples at parent node (e.g. 60% Yes 40% No)
    
9. Probability: Based on the ratio of examples at parent node
    

10. If there are a mixture of positive and negative but no more features, then the remaining examples have the same description, but different classification.
    

11. Could be due various reasons, including:
    

12. Noise in the dataset
    
13. Unobservable features
    

14. Can solve using Plurality classification
    

### How do we choose the best feature?

The goodness of a feature is based on the amount of information it provides about the classification.

- How can we measure this?
    
- One way is by looking at entropy.
    

#### Information gain and entropy

Entropy: A measure of uncertainty.

The entropy of a probability distribution <P1, … , Pn> is:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcO0hETWH1kkNUj_VA8UnhmuTHVRQDqMgI9oNiitVI16mVpPsVOpNH2FP9pF4a05I8PErHkiAlFEdn3TIX0robc-db1l_vAcW1ttt5LUHOMf5zPe8a2K8XwX1v6efk-TZTQDctM2Q?key=DAssY80HPaH4Gt4ccOL2V3vW)

Or the sum of the (negative) probability multiplied by log two of the probability, for all probabilities in the distribution.

This is greatest for a uniform distribution, as we are more uncertain about which class is the likeliest, and smallest for a single point, where the uncertainty is zero.

##### Small calculator tip

log2(x) = log10(x) / log10(2)

##### Calculating Entropy from Examples

We can calculate entropy based on positive and negative examples. Let p be the number of positive examples, and n be the number of negative examples. The probability of a positive example is:

p / p + n

and a negative example:

n / p + n

Thus, we can calculate the entropy as:

H(<p / p + n, n / p + n>)

  

For the previous example of the 12 restaurants, p = n = 6, so the entropy is 1.

  

This is for only two classes; but if you have n classes, you just have to calculate the entropy for all the classes.

##### Using Entropy to Choose Features

To choose a feature, we measure the reduction in entropy from the current set to the next set, called the information gain. Specifically, we want the feature that results in the lowest entropy, or the highest information gain.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd7P_7Cv-y0iz4E5yxkdPL8PrfgT6LHXJexkAzcP-B0g1FGin5h8XoAbijTRv6Wx8g1MHwDnDBIO_Z0Xehjvmtw8fyH8aOcHEdWReM_SkhiRUSTvHXjdiWb24fDsxDAWg1NJpS_0g?key=DAssY80HPaH4Gt4ccOL2V3vW)

Formula explanation: A feature will split the set of examples into a number of smaller subsets, each with their own entropy. So, we sum the entropy of these subsets, weighting each of them with the factor |Si||S| (the number of elements in Si over the number of elements in S).

##### Example

Going back to the previous Restaurants example, Patrons results in an information gain of 0.541, whereas Type results in a gain of 0. Thus, Patrons is a better feature than Type.

##### Downsides of Entropy/Information gain

The information gain measure favours features that have many values. For example, a feature that split each example into its own group, like ‘Restaurant Name,’ would create subsets with an entropy of 0, for the highest information gain. According to information gain, this would be very good, but would be useless for actual classification purposes.

##### A Fix

Add to the formula to penalise features with lots of values, in the form of a metric SplitInformation:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfOyFUX0hUDP8sMHSHEIwhAKRsAXZEgeivgVTQUk65RdenRfLuLMBEIXklnA6SO9tADpzWlenq9c1MLuRo4Dawyy0Twd-TreWvocguFPg-818G5VqMZa_YU86-QyNbKHCm-tzhD?key=DAssY80HPaH4Gt4ccOL2V3vW)

Then, use the Gain ratio instead.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeVEuZE_RWqFL6T0g_6ZNW-XiZvxJOD44B4fxDMoVDYzl2MHRGQGvv6FSAe-DRDZvCqAh2dygia7zCYVvQBxXyWIYlyhzTijn8yz36jPgDrNVH8w1x7WYa0y5d5_KBqXolurYZKIw?key=DAssY80HPaH4Gt4ccOL2V3vW)

The information gain divided by how much we split the set into subsets.

#### Gini Impurity

An alternative to entropy, where we assign probability based on the number of examples:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcqBMGUF1AJkCvYt_7vIPIQPqB6MvLFz6C0buYYhOGFX8bTwqg1rjXsoG2l-O67XsIqEZuLwHVPdDdEXEfpMFbH1gAI0r9pmURzYJb5ZawBywrGu6ZoS63VDxn3nOC4KdiWAyihuA?key=DAssY80HPaH4Gt4ccOL2V3vW)

Gini impurity is the probability of this classification mislabeling a randomly selected example.

The general formula for Gini impurity when there are c classes is:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcIjmNtLujruxy-4v_th0bnHzOLN_wS73_HB4wpj65f0Y73jwU-BVF4oWAjG0UK9uAkgpHXrP625yOwtt6Pjo8Aikkt3A_ScPJyNiJgeqs_enlP9K2jgI-I4jTT4Y88My62lw6fmw?key=DAssY80HPaH4Gt4ccOL2V3vW)

or:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfOBpCTq-Q7_FogkvMcJmD-wuwW5arKVNwbkiqCz9I1QxJqvtLrvkPwFXLSZpO4lLfxENmR0OMZhrJuM1z59Awz87f1vLQeGtoCBIKfgUJX4JmHWOhyHfOecnGXKjMr1r81ktfzPw?key=DAssY80HPaH4Gt4ccOL2V3vW)

An example, for two classes:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfQHlWcp4vxUNSTLdC-3P3n0UvZaUMCc86d2S_sm-sivxqNKXIcCi7_1DLtfQrWBZtGy_uZ4x2cpzDPSRwHL777e5VfWE3F-BWIaWnPmD2adDLJltVBZetTa8FMAuUjILb5q1gNYg?key=DAssY80HPaH4Gt4ccOL2V3vW)

##### Using Gini Impurity

For a feature A that splits the set S into Si subsets, the Gini impurity is:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeN4d6xpG7vNzbXqwq8IkTuTju3lNdXHiFuDmOrvp7Gohw6cyEJ12RuPZD_adyNcsxWpED8l-1CSTORX3pSf8rRw6tjW0f6wMiY2EGZ4mhZ5npmukVoOtsVZydN5uwD5kqLTNL2FQ?key=DAssY80HPaH4Gt4ccOL2V3vW)

To pick a feature, you calculate G(S,A) for each feature, and choose the one with the lowest Gini impurity.

### Overfitting

Decision Tree Learning grows the tree just enough to perfectly classify all the examples.

If the set is too small (or contains noise) then this can easily lead to overfitting.

A decision tree d overfits if:

- It has a small error on the training data, but
    
- a large error on all other instances (i.e. generalises badly)
    

#### Preventing overfit

Two methods:

15. Stop growing the tree before it overfits
    
16. Allow overfitting, and then prune the tree.
    

Let’s try the second one :)

#### Reduced Error Pruning

17. Consider every branch in the tree as a candidate for pruning.
    
18. Remove the branch, and make the root of the branch into a leaf.
    
19. Use plurality classification for examples at that leaf.
    
20. Test the new tree:
    

21. Keep a branch pruned if the pruned tree performs better on some validation set than the original tree.
    

22. Repeat while it is possible to prune a branch and improve performance.
    

However, this can be difficult if our dataset is very small, as we want to maintain partitions as before:

- Training data
    
- Pruning data (a.k.a. validation set)
    
- Test data
    

So, an alternative solution:

#### Rule Post-pruning

23. Build the decision tree as before
    
24. Create a rule set, one rule for each path from root to leaf.
    
25. Remove preconditions (feature tests) from rules if that improves their accuracy.
    
26. Sort rules by accuracy and apply them in order when classifying examples.
    

##### Example

Decision tree:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcsgZfLR-puL9uSxnWeMkmKRTKrK94nVFcN7MNfVbdFuPOItJ2J-yS1-GVHLPiY8Ka7JDm1KXe4Zn12pRy-TJQP90oTK_Us2BPB-o36OkZQ4zZ5Dt0EiD58-t5XTlipxPXuLrgpgw?key=DAssY80HPaH4Gt4ccOL2V3vW)

Generated rule set:

Notice how each rule corresponds to a possible path.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcsgZfLR-puL9uSxnWeMkmKRTKrK94nVFcN7MNfVbdFuPOItJ2J-yS1-GVHLPiY8Ka7JDm1KXe4Zn12pRy-TJQP90oTK_Us2BPB-o36OkZQ4zZ5Dt0EiD58-t5XTlipxPXuLrgpgw?key=DAssY80HPaH4Gt4ccOL2V3vW)

Then, for the second rule, we can try removing one condition or the other, and see if it improves performance.

  

A benefit of this method is that rules are more flexible than branches, as we can remove the root while maintaining lower branches.

### Broadening Decision Tree Approach (Optional?)

- Multivalued features
    

- When features have many values, it screws up our information gain measure
    
- So, convert stuff to boolean tests
    

- Continuous / integer input features
    

- Infinite sets of possible values
    
- Modify our approach to identify split points which give the highest information gain.
    
- Basically if x > 10 else if x <= 10 or something
    

- Continuous output values
    

- When trying to predict continuous output values, we need to create a regression tree, which ends with a linear function
    

## Linear Regression

(Univariate) Linear regression = 

hw(x) = w1x + w0

Where the subscript w indicates the vector [w0, w1]

We want to estimate the values of w0 and w1 (weights) from the data.

### Picking Weights (Loss Squared)

We need to find values of w0 and w1 that minimise the loss - that is, the distance between the line and the data points.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdFPNGxgPtZHQmg2_PjvOCMO3W87URKOUX9Diow8A8PQtZEPZW1e7MPO3htr1KevMDRsDCWA6wcdvXljeIu3sUicKkEMg_tjnzw8-cBxVuwcN5VIF9nGnGdL01t28BsiF0Dd7pv2Q?key=DAssY80HPaH4Gt4ccOL2V3vW)

To measure this, we can use the following Loss squared function:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXceF0sjJl7PpickeBQGFUaQe_LQIHoHUcygXJc_WFUh3WQ4tUEIsq6-rZbFZGOg_CvD2K3VqFDV30rTXFth5d2dG_8PRLBYub0d3Q8LmNZCAFrDJ0IurFGwNE7DGMsswpXeQuZimA?key=DAssY80HPaH4Gt4ccOL2V3vW)

This formula looks complicated but in reality, all it does is take the distance between each data point and the line, squares it, and adds them all up to get the total loss (squared).

So, our goal is to find some w* that minimises Loss(hw*). Or in annoying math notation:

w* = arg minw Loss(hw)

  

Why use squared loss?

Because for normally distributed noise, squared loss gives us the most likely values of the weights.

### Squaring the Loss Function

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd-F-CxV6qGUIL67F8dlp1hANPvpM63FEDna8wkwXFHYZKqw6TmjIv34nyF4J_KMrWjSiU5Kg-FFD6KcfKO9HQ6YbRzqaBtJOku1sqEdNQ-oxUwueW2qia1u9MdixZwBfmOxK6BMw?key=DAssY80HPaH4Gt4ccOL2V3vW)

Plotting the weights against the loss function for univariate linear regression, we get a graph like this. We want the value of w that minimises the loss function. (The above graph assumes only w1, and no w0.) The following is the same graph, but including w0:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfdYoU55UP2oP1CJFausYPc7fUy1Mr9A3Fj6OpMoO9erEqYZjzhCdSMCMP-B_3FQALTGS4c2tOXEsC_kgrL32VflsEfPlZRrsJ7Zgy2NTJExupPdcUtmzclNm8zOiSDAv5D7XBX?key=DAssY80HPaH4Gt4ccOL2V3vW)

We want to find the minimum of our loss function. To do this, we use gradient descent.

### Gradient Descent

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeZagF71zygFtLviTmDhXfiuP3mWeSgJvwfn81aFP41yS5C9J04XY0QshCnTm3SSkGuT7L4LiTvkLn_TjKnFXNUqva6KIz09DNdC5cEaA9yHo4H893cKmtN9cuU60nUcs6S7Vmvpg?key=DAssY80HPaH4Gt4ccOL2V3vW)

27. Start with some initial values for w0 and w1. 
    
28. Keep changing these values so that the loss function is reduced.
    
29. Continue until a minimum is reached.
    

In more detail:

30. Start at any point in the (w0, w1) plane and move to a neighbouring point that is downhill.
    
31. For each wi, we update with:  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe2JlY_nXn_tcDUilkvxTisrua2KXQpRipOzOGmrvKYBwGlzjH8uv2zk9FZmjHQw-galIKhx0TH1GmGyRfNh1fRxaCsrUm9eLxa5WLeRzqU0iL2AiG_Bd7ZWryBQrF15z5KosTCEA?key=DAssY80HPaH4Gt4ccOL2V3vW)
    

Where:

32. Alpha is the learning rate, controlling how big of a ‘step’ we take.
    
33. The part after alpha is the partial derivative of the loss function with respect to w. Or, the gradient of the loss function at that point:  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdVG92dBA5D2e3areah4bR_AHu4ilbmKgEkNXx8XskuDih0XPYsPkKrZGwbw1OyqnRkVodFvwrzgv6lk5Jf3QnSAQk6NPaxG5jjQDxEPB2JPC6Dq0Ob7ipkBTMapJ3OEEbGtC3WjQ?key=DAssY80HPaH4Gt4ccOL2V3vW)
    
34. So, a positive gradient will push us to the left, and a negative gradient to the right (as it is subtracted)
    

35. Repeat until convergence.
    

The slope at the minimum is 0, which means that the partial derivative of Loss(w) = 0. In other words, the value of the loss function is minimised when the partial derivative with respect to w0 and w1 is 0.

Note: Must update all weights simultaneously

#### Learning Rate

Above, we described Alpha, the learning rate, which controls how big the ‘steps’ we take are.

If Alpha is too big, we risk overshooting the minimum and not converging.

If Alpha is too small, then it will take a long time to converge.

### Types of Gradient Descent

#### Batch Gradient Descent

Consider all training examples simultaneously when deciding our weights.

Guaranteed to converge, but can be quite slow.

#### Stochastic Gradient Descent

Update the weights using one example in the training set at a time.

Quicker than Batch, but might not converge, especially if learning rate is constant.

## Multivariate Linear Regression

We now have more variables

So, hw(xj) is the weighted sum of the variable values:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc20GGcrq-lvJqzBne7UZm04MtmuleLMKsHZ6hJ3OmgFlh5mG_Dtgcc95Q_Dcyvlol8tdQFD5YmSPE1kbp8Snv3zUjCtdxYIRhbJY2_GC2OneZeMZ7O69xNvSofv9OjoACToTy1oA?key=DAssY80HPaH4Gt4ccOL2V3vW)

(Add xj,0 as 1 so we don’t have to treat w0 separately)

### Overfitting

Now, we have to worry about overfitting, because we have a more complex model (with more parameters)

  

To handle this, we can add a regularisation term

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfPg5Kby1Xll0DbCeJdYNcXXOz0lI9J8m70s9LqzQSTLOSo8EpakRSPPGSz2IxnbCvi-l4cbK2G1_RJbeGqByqTyJy6Gw4yvXe-7t5kXmTXuP2dUzw_PrUTI2d-TQO2Fajf8_Nn?key=DAssY80HPaH4Gt4ccOL2V3vW)

where:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcktMzRTQnSjIIeiTwOWAeLEvTOcJ1fJxbbPdwya6hdrkATI0ByAqalrZCpt3W18AoJgIOcZtm3-U2UMrKZAraNHkgfI0V5B9FlYZX41GSpiMhlsu7cpk4oFAzxJ4jFj3ZbJdbZ?key=DAssY80HPaH4Gt4ccOL2V3vW)

Smaller w will result in simpler functions.

Lambda is the regularisation parameter - the trade-off between fitting the data and having a simple function. What if it was too high?

We use Loss’(h) when computing the weight updates.

## Linear Classifiers

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcSvvva_wMolSdtPHP3uQMwfa9AUBUi9M4gU2462m3pb2h_DHWUEJ7b27gaEH9rRHPM4CObzezNaxcDl_SuKAANu-TWtkTcJ3EitgdlkPAkTQYNken2SkwilcjV5K2lY9CgAtV4?key=DAssY80HPaH4Gt4ccOL2V3vW)

Linear regression fits a line to data - linear classification uses a line to split data.

A linear decision boundary works if the data is linearly separable

  

Decision boundary is given as a linear equation, for example:

  

y = 1.7x - 4.9

which can be rewritten as:

1.7x - 4.9 - y = 0

  

For things to the right of the line:

1.7x - 4.9 - y > 0

  

Thus, classification happens as follows:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfJParl3UbbQgHh_nyna2o4IizrURoaFLlEJNt29wpywPvhCxPaWu2NRaOz5qDPn9JOR9aNv24tSAxkGwpk4lsrkm_SOG_6If4I42OE2qONONhWnQGvjdpDlST5JJW_GSfhEZ-PPQ?key=DAssY80HPaH4Gt4ccOL2V3vW)

### Learning Parameters

We can learn the weights the same as for lin reg.:

- Start with arbitrary weights.
    
- If it classifies correctly, don’t update
    
- Otherwise, update as before (gradient descent, etc.)
    

Common to use stochastic gradient descent here.

### Learning Curve

This is what the learning curve may typically look like:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf6zKJeK4hWWYE0rl-EGR_ICFFfamwcq8KseXc_yFqUVdIFNAx6HNK2io3_4eo3d9gW170R2jQ0-DJ6-6rYEjzv-rqshA6QQ181fTudPYxD4CY7waNUBtpD0jrBN6hi5Ps6t08Daw?key=DAssY80HPaH4Gt4ccOL2V3vW)

Note that the curve is not smooth because the boundary is hard; so a lot of examples can be misclassified, even a long way into learning.

The curve may look even worse if the data is noisy:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXePxfNZO992ZRH06W-hdo3MwCpgiKz_rmFaWGS8mrpvbydznrU0-XGK1XARV1R6Yd4Gv4A36t_omHTAdxftqAVgAnVUI1UVlx0HJsonBmOpFYRHIHaxCdh2TpEKvnE19iAHJ_Qvyg?key=DAssY80HPaH4Gt4ccOL2V3vW)

One way to deal with this is a soft decision boundary.

## Logistic Regression

Instead of predicting 0 or 1, we can ‘soften’ the function by approximating the hard threshold with a continuous function.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdZTbGqay7lfeQFk88MpLJNVWgI2MZSxs--ppZmOk_zmtJQps_RzN3kE9pWF7xoVr6lAJB5lLAWK3okB85byU9cCTbnkCRDsvWyOFh4YobV6TAAUGBlwdUfJqfzRNQwhk4WkZyiFA?key=DAssY80HPaH4Gt4ccOL2V3vW)

### Calculating weights

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdoYz4OIjnaqalw1PXdfW3vsBIrhk9GuxSP0I7plf2oTRZ2OZV1iV42WCh6thGSlrPS27nzyhShDhtnineWO0XH85cABJJCaAI-tkB4GhpGBY-2wzK6ETfjc6q9EeKt095Czc46?key=DAssY80HPaH4Gt4ccOL2V3vW)

### Learning Curve

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfw2MEz69krN0kp-Oq8xovxkH6SyRbunY6lsIC3u-33aHhhJvFqzPxrizEWbfuwoUMfdMifFrDYFWIsMu60XdcuvXORGOJsihqz1Q4KzL0khLRPpIa3uONVFowVoDIQPxX_yB4A?key=DAssY80HPaH4Gt4ccOL2V3vW)

Much nicer :)

Slower convergence, but more predictable

## Issues with Linear Separability

..?

## Ensemble Methods

Every classifier has an error rate - i.e. every classifier will always misclassify some examples.

We can improve on this by using ensembles - using multiple classifiers on the same example, and have them ‘vote’ on the classification.

### Boosting

Use a weighted training set, where weighted examples are counted as ‘more important’ during training.

  

- Start with all examples of equal weight, and learn a classifier h1.
    
- Increase the weight of misclassified examples, and learn a new classifier h2.
    
- Repeat.
    
- Final ensemble is the combination of all the classifiers, weighted by how well they perform on the training set.
    

### Boosting Variants

#### AdaBoost

Commonly used approach to boosting

Given an initial classifier, AdaBoost generates an ensemble that perfectly classifies the training set.

#### Bagging

Take an initial training set, turn it into multiple different training sets. 

From each training set, learn a classifier.

To classify an example, combine the classifiers and use as an ensemble.

#### Combining results

Typical combination of classifiers is by voting, can also combine regression ensembles by averaging. Both can be weighted

### Random Forests

Bagging applied to decision trees

### Random Subspace Method
%% to do later%%

[[3 - Probabilistic Models 1]]