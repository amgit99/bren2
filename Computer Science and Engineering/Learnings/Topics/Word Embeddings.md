The most popular quote in this context is "A word is known by the company it keeps". Taking this assumption we make a co-occurrence matrix this is a $|V|*|V|$ sized matrix where $V$ is the vocabulary. This stores the counts where the words $i$ and $j$ appear within a window of 5 words.
Taking the SVD of this co-occurrence matrix reveals the latent semantics of the words.
[[Singular Value Decomposition]]

#### CBOW model
We use neural networks and the task is to take a 3-4 word context and predict the next word. This is based off of the markov assumption, which states just a context of 3-4 words is enough to predict the next word.