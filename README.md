# Quora-Insincere-Questions-Classification
This repository is for the Quora insincere questions classification competition in Kaggle.


**45th Solution**
![rank](img/rank.png)

<!--Finally this 2-month journey has finished, this is actually my first Kaggle competition I attend end to end. So excited to got the rank 45th and my first medal in Kaggle! 

I just try to write down all the timeline and procedures I followed in this journey, and organize all things I’ve learned from this competition.-->

## Some key points in this competition:
 -  **The tradeoff  under the limit of time**: Since the competition limits the kernel run time in 2 hours(for GPU user), that means some tradeoffs need to be considered for every competitors. E.g. the tradeoff between tuning a single model and blending different models; the tradeoff between different architecture (RNN generally perform better but CNN’s is easy to parallel which makes it run much faster on GPU); the tradeoff of information completeness and the dimension for different ways to deal with embeddings (Mean vs. Concatenation)
 
 - **The validation choice**: For this competition, I think many Kagglers have noticed that there is a significant difference in the performance of local cross validation and test data. At first I did use the Public Leaderboard to validate my model, but after several submission trials I noticed the model which has a significant better CV would not perform well on Public Leaderboard. Considering the data size (1.4m in Training, 70k in Public Leaderboard and 350k in Private Leader board which is used to evaluate the model eventually), I decided to only look at the CV score.

## Key Strategies
- **Find the optimal point in the limited time.** Due to the time limit, I gave up to find a “global minimum” for the model, but aimed to find a “local minimum” for each model, so different learning rate scheduler has been tested and take both time and performance into accounts. In the final model I used Multistep Learning Rate. Scheduler, each step are tested to make the model converge within 4 epochs.

- **Try to utilize all the resources to best explore the diversity.** In the final model, I used four different embeddings: Glove, Fasttext Paragram and the mean of those 3. Also a bit changes made in the model architecture to fit the embeddings.
- **Use multi-processing module to accelerate the text preprocessing**, which save me about 20 minutes to be utilized to add another for blending.
- **Smartly Optimize the F-1 Score.** The metric used in the competition is F-1 Score, which could not be directly optimized. Like other teams, I used two stages to approximately optimize it, I optimize the log loss firstly then tried to find a optimal threshold to make hard classification. But one difficult thing is, small validation set are needed to find the “best threshold”, and this threshold could vary significantly for different training. So my strategy here is to weights the loss function, by setting pos-weight to 1.8, my goal is not to optimize the loss function, but to make the threshold more stable. In this way the “best threshold” could be stabilized at ~0.53, which is used for my final models.


## Model
The model I used is basically based on bidirectional LSTM and GRU with attention, with different embeddings and features including:

-  The number of capital characters in the sentence.
-  The number of words that are all made of capital characters.
-  Mark If the sentence has misspelling.
-  Mark if the sentence has special symbols.
-  Mark if the sentence has contractions.



## Things I learned from this competition:

* A lot of techniques in NLP, e.g. embedding, tokenization, sentences preprocessing, and feature extracting.
* Neural network in language modeling, better understand the embedding layer.
* Know how to use multiprocessing module to accelerate the data preprocessing.
* Better understand for attention mechanism.
* Better understand for tuning neural network.


## Things could have done better.

* Modeling directly in Kaggle kernel could be really unstable, should have deploy my own environment.
* In attention layer, I mistakenly put the mask after the softmax, should have checked the self-defined attention layer, (After rectify this error, the score would be 0.003 that could earn me a gold medal)
* Learned from top winner solutions, more feature engineering work could be done.