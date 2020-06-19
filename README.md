# LGM-EmbeddingToponymInterlinking
This repository contains the experimental code for learning FastText embeddings on toponym pairs and exploiting them as toponym representations in the toponym interlinking problem, formalized as binary classification task.

## Dataset
The dataset available in 'data' folder is extracted from the [Geonames](https://www.geonames.org/) database. It consists of the following files:
- **_train_**, containing 2M toponym pairs
- **_validation_**, containing 500K toponym pairs
- **_test_**, containing 2.5M toponym pairs

The labels ratio is balanced for all three subsets (50% True - 50% False pairs).

## Method
- **_Preprocessing_**
  - Each toponym is first normalized by applying a series of preprocessing steps e.g. transliteration, accents and punctuation removal, digits filtering.
- **_FastText embeddings_**
  - We build a training list that contains sequences of toponyms tokens in order to train a FastText embedding model.
  - More specifically, for each matching (True) toponym pair *<T1, T2>*, we split each toponym into tokens, producing two lists as *\[T1_t1, ..., T1_tn]* and *\[T2_t1, ..., T2_tn]*. Then we add to the models training list both *\[T1_t1, ..., T1_tn, T2_t1, ..., T2_tn]* and *\[T2_t1, ..., T2_tn, T1_t1, ..., T1_tn]*, as this design choice was found to lead to better results.
  - We train a FastText model on the above training input to come up with a model able to map each toponym to a semantically rich dense representation, useful for the interlinking task.
- **_Classification_**
  - For the classification part, each toponym pair is representated as the concatenation of the representations of the pair's toponyms.
  - We utilize a basic feed-forward neural network in order to perform the interlinking task, formalized as a binary (matching or non matching toponyms) classification task.
