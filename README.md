# W207 Final Project

Kaggle: [Personalized Medicine: Redefining Cancer Treatment] (https://www.kaggle.com/c/msk-redefining-cancer-treatment)

Predict the effect of Genetic Variants to enable Personalized Medicine

### Update 08.16.2017

**Directory**

- EDA.ipynb
	- Exploratory data analysis of train and test data
- Preprocessing.ipynb
	- Tokenization
	- Custom Preprocessing
	- Dimensionality Reduction
- Model_1.ipynb
	- Best performing model
- Hypothesis_1.ipynb
	- Alternate hypothesis
	- Includes feature engineering of `Variation` variable
- hypothesis_keras.ipynb
	- Original hypothesis notebook


**Notes**

- Organized repository to keep notebooks containing assignment details, presentation and notebooks separate.
- To aid in readability, notebooks have been modularized into EDA, Preprocessing and Model components

**Results and Discussion**

Early results submitted to Kaggle with a multiclass log-loss score of `0.82386` have been difficult to match with other strategies and model optimizations. 

After talking with a domain expert working on a Phd. in molecular biology, the strategy for improving model results was refocused on the information carried in the `Variation` variables themselves. Amino acid changes are coded according to a set of [guidelines] (http://www.hgvs.org/mutnomen/codon.html) and different types of protein mutations are more likely to result in certain types of outcomes. Different amino acids also carry different properties of phosphorylation and charge which are meaningful to protein function. So, for example, it is probably important if the amino-acid change was to or from a threonine (Thr, T), serine (Ser, S), or tyrosine (Tyr, Y) as these could have more likelihood of loss or gain of function (which is broadly the definition of the dependent variable in our classification). 

The realization that each value in the variation field carries more information than just a single class representation seems important for this task. If we look at a randomly selected variant, say `V1075Yfs*2`, we can break that down into:

- V = Valine
- 1075 = position in gene
- Y = Tyrosine
- fs = frame shift
- *2 = reference of frame shift

Furthermore, [each amino acid has properties] (https://en.wikipedia.org/wiki/Amino_acid) we can include:

Valine = aliphatic, nonpolar, neutral charge
Tyrosine = aromatic, polar, neutral charge

Given this information, a non-trivial amount of time was spent programmatically feature-engineering the values of `Variation`. Each element of the protein-level mutation was broken down into components and added as a one-hot-encoded feature. Additional properties for each amino acid were also included, with different values for before and after the mutational change. 

The EDA reveals some of the complications of this process, but since most observations of the variation fit the standard nomenclature, this seemed like a reasonable expenditure of time. Unfortunately at the time of this final update, including the results of the feature engineering only made overall model performance worse. 

This approach is documented in the notebook `Hypothesis_1.ipynb`. 




### Update 07.25.2017

**Limited Data**
Although there is so much text data as to be computationally challenging, it is noisy and difficult to parse given the limited labeled data.

**Tokenization**
Finding that tokenizing is challenging due to vocabulary size. Tri-grams result in over 20 million features -- even when limiting CountVectorizer to top 10% of terms. Currently using bi-gram tokenization (~ 2 million features).

**Dimensionality Reduction using Singular Value Decomposition**
SVD and PCA are generally effective strategies for reducing dimensionality, and for sparse matrices (like TfidfVectorizer objects), TruncatedSVD is the preferred method. In order to select an optimal value for the number of components (`n_components`), several functions were built to analyze the hyperparameter's impact on explained variance. Because of the processing time, but also as a general strategy, the training set was sampled without replacement and each run was plotted with a range of values for `n_components`.

For samples that were approximately `8%` of the training dataset each, a point of diminishing return was observed starting around 20-40 components.  General guidance for using TruncatedSVD suggests `n_components` be set around `120-200`, and if we scale `20` features by an order of magnitude (to account for the size of the training set), we arrive in the same ballpark. Considering that scientific literature may contain many domain specific terms, it seems logical to err on the higher side of this estimate.

The current submission score on Kaggle's leaderboard using this model is `0.82386` (multiclass loss), which ranks `274` of `531`.

**Considering the following strategies:**

- parsing external data sources that might allow the model to better learn a mapping between vectorized tokens and the output classes.
- expand on sampling used in SVD analysis to provide a bootstrap approach to labeled validation
- explore ways to deal with class imbalance, which is significant in training data.
- Neural network architectural improvements
- Learned embeddings using neural network architecture.
- CNN-LSTM network as a way to identify patterns beyond bi-grams
- Improved latent representations of text data


### Update 07.12.2017
Added basic tfidf with neural network classifier. So far only an accuracy on the test data of “0.3234”.

TODO:

- eda
- model exploration
- metrics
