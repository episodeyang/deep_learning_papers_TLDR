[–]egrefen 17 points 1 year ago 

Before I answer, I should point you to the interesting comments and biblio references offered by /u/davidcameraman. I should extend his point about how various embeddings methods perform similarly on a variety of NLP tasks by citing

>  Levy, Omer, Yoav Goldberg, and Ido Dagan. "Improving distributional similarity with lessons learned from word embeddings." Transactions of the Association for Computational Linguistics 3 (2015): 211-225.

and

> Levy, Omer, and Yoav Goldberg. "Neural word embedding as implicit matrix factorization." Advances in Neural Information Processing Systems. 2014.

which I believe are two of the most seminal papers on word embeddings in the last decade, in that they relate research on word embedding models to the fairly large body of research on **distributional semantics (Firth/Harris)** from the last 50 years, and proceed to show that all of these approaches are effectively equivalent in performance and representational power, up to the correct choice of hyperparameters.

With that said, I should say that I am not that interested in embeddings. This may seem like a strange thing to say given my previous line of research in **compositional distributional semantics**, and my current research in recurrent networks applied to NLP, so I will attempt to elaborate.

First, let me state what I think word embeddings bring to the table. They are, in my mind as well as that of my colleagues, in no way a good general representation of semantics, but rather just one very successful example of an application transfer learning between **contextual prediction** (word given context, or context given word) and other domains with very different objectives (sentiment analysis, language modelling, question answering), either by serving as representations in their own right, or as initial settings to aid training. Furthermore, pre-trained embeddings are also very useful when training models in domains with little data, where the proportion of out-of-vocabulary words in test and validation with regard to the domain-specific training data is high (above a few percentile), in which case using (fixed) pre-trained embeddings as word representations is a suitable compromise.

Now let me say why I don't really care much about embeddings. The first reason is purely empirical: with sufficient data, you just don't need them. In fact in many cases, **they may hinder both training and model performance**. A simple artificial example is as follows: a word embedding model may plausibly project words like "car" and "automobile" into the same segment of a semantic space. Yet consider the task of classifying text based on lexical register: you will in this case wish to detect, say, the distinction offered between using "car" and "automobile", as a basis for differentiating high register language from more colloquial language. In such cases, the representation similarity brought to you from the objective that yielded the word embeddings actually makes your primary model's life harder than initialising the word embeddings randomly (and hopefully pseudo-orthogonally) and then training them from scratch, rather than having to learn very fine spatial boundaries over the input.

The second reason is more conceptual: in the case of recurrent networks, I tend to see the embedding matrix as part of the network itself, allowing it to consume discrete input symbols encoded as one-hot vectors, and updating the state of a recurrent cell. In this sense, embeddings are just weights of a linear transform from the one-hot input into vectors used by the network's internal dynamics. **Meaning and interpretation, if there is such things, are present in the state of the network, rather than solely in the embeddings**, and it makes as much sense to seek to interpret the weights that constitute embeddings as it does to seek to interpret any other weight in the network. Pre-training embeddings and using them in another network, under this view, is even more explicitly just a form of transfer learning, in that we are initialising the weights of part of a task-specific network, and perhaps freezing them, with information obtained from another task. It's not a bad strategy, but I think people focus too much on this very specific form of transfer learning rather than, more generally, on other options there are out there (or yet to be discovered) to help us deal with data paucity, and to best share information across similar tasks.

Anyway, I hope this perspective makes some sense to you and goes a little way towards answering your question. Thanks /u/nandodefreitas for suggesting this question to me :)