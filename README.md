# W207 Final Project

Kaggle: [Personalized Medicine: Redefining Cancer Treatment] (https://www.kaggle.com/c/msk-redefining-cancer-treatment)

Predict the effect of Genetic Variants to enable Personalized Medicine

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
