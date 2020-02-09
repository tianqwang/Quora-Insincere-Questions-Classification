# Quora-Insincere-Questions-Classification
**45th Place Solution** of the Quora insincere questions classification competition in Kaggle.

[Quora Insincere Questions Classification](https://www.kaggle.com/c/quora-insincere-questions-classification)

![rank](img/rank.png)

<!--Finally this 2-month journey has finished, this is actually my first Kaggle competition I attend end to end. So excited to got the rank 45th and my first medal in Kaggle! 

I just try to write down all the timeline and procedures I followed in this journey, and organize all things I’ve learned from this competition.-->

## Key points in this competition:
 -  **The trade-off under the limit of time**: Since the competition limits the kernel run time in 2 hours(for GPU user), that means some trade-offs need to be considered for every competitor. E.g., the trade-off between tuning a single model and blending different models; the trade-off between different architecture (RNN generally perform better, but CNN’s is easy to parallel which makes it run much faster on GPU); the trade-off of information completeness and the dimension for different ways to deal with embeddings (Mean vs. Concatenation)
 
 - **Validation strategy**: For this competition, I think many Kagglers have noticed that there is a significant difference in the performance of local cross-validation and test data. At first, I did use the Public Leaderboard to validate my model. Still, after several submission trials, I noticed the model which has a significantly better CV would not perform well on Public Leaderboard. Considering the data size (1.4m in training, 70k in Public Leaderboard, and 350k in Private Leader board, which is used to evaluate the model eventually), I decided to only look at the CV score.

## Key Strategies
- **Find the optimal point in the limited time.** Due to the time limit, I gave up to find a “global minimum” for the model, but aimed to find a “local minimum” for each model, so different learning rate scheduler has been tested and take both time and performance into accounts. In the final model, I used the Multistep Learning Rate. Scheduler, each step is tested to make the model converge within 4 epochs.

- **Try to utilize all the resources to explore the diversity best.** In the final model, I used four different embeddings: Glove, Fast text Paragram, and the mean of those 3. Also, a bit of change made in the model architecture to fit the embeddings.
- **Use a multi-processing module to accelerate the text preprocessing**, which saves me about 20 minutes to be utilized to add another model for blending.
- **Smartly Optimize the F-1 Score.** The metric used in the competition is the F-1 Score, which could not be directly optimized. Like other teams, I used two stages to approximately optimize it, I optimize the log loss firstly then tried to find an optimal threshold to make the hard classification. But one tricky thing is, a small validation set is needed to find the “best threshold,” and this threshold could vary significantly for different training. So my strategy here is to weights the loss function by setting pos-weight to 1.8; my goal is not to optimize the loss function but to make the threshold more stable. In this way, the “best threshold” could be stabilized at ~0.53, which is used for my final models.


## Model
The model I used is based on bidirectional LSTM and GRU with attention, with different embeddings and features including:

-  The number of capital characters in the sentence.
-  The number of words that are all made of capital characters.
-  Mark if the sentence has a misspelling.
-  Mark if the sentence has special symbols.
-  Mark if the sentence has contractions.



## Things I learned from this competition:

* Many techniques in NLP, e.g., embedding, tokenization, sentence preprocessing, and feature extracting.
* Neural network in language modeling, better understand the embedding layer.
* Know how to use a multiprocessing module to accelerate the data preprocessing.
* Better understand for attention mechanism.
* Better understand for tuning neural network.


## Things could have done better.

* Modeling directly in Kaggle kernel could be unstable, should have deployed my environment.
* In the attention layer, I mistakenly put the mask after the softmax, should have checked the self-defined attention layer, (After rectifying this error, the score would be 0.003 higher that could earn me a gold medal).
* Learned from top winner solutions, more feature engineering work could be done.