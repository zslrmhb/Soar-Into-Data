# No L’s in LLMs
> Analyze AI-generated responses by engineering linguistic features, readability, and semantic similarity to predict winning models and improve prompt-response interaction quality.

[DevPost Link](https://devpost.com/software/no-l-s-in-llms#updates)

## Project Struture
- feature_metadata.txt: metadata for features-v3.csv
- features-v3.csv: features used in training
- final.ipynb: the code for the project

## Introduction
With the ever-developing Large Language Models available nowadays, we tend to be more reliant on these models to provide information that we are interested in. As developers, ensuring the LLM delivers accurate responses is critical. However, from a business perspective, we want our model to attract the most users out of the model, and one way of doing that is to understand the users’ preferences and expectations. So what qualities or characteristics in chatbot responses make them more preferred than others?

## EDA
 Our train dataset contains about 57000 entries with each entry containing a prompt the user inputted, two responses from two different LLM versions, and user’s preference on either of these two models or if it is a tie. There are 64 unique LLM version used, and some get chosen more often than the others. From the win distribution, we found gpt-4-1106-preview to be chosen more than the others. In terms of win percentage, three versions get chosen more than 50% of the time when given the option: gpt-4-1106-preview, gpt-3.5-turbo-0314, and gpt-4-0125-preview.

Our first thought was to check if length of response matters to the user. Running some insights from data, we found that no matter the position of the response, the winning response tends to have a longer response compared to the other response.

But does the position itself matter? We dove deeper into whether a user is more likely to choose the first response they get. To get some ideas, we tried to get the winning distribution for each LLM version per position (A or B). When we average them out, we found that position does not play a significant role in determining user preferences. The difference between the average win distribution is only 0.06.

 ## Feature Engineering
 ### Features Categories

 #### Sentiment Score
 - [VADERsentiment Analysis](https://github.com/cjhutto/vaderSentiment)
 - Sentiment score ranging [-1, 1] for negative to positive sentiment

 #### Cosine Similarity
 - [SentenceTransfomer](https://huggingface.co/sentence-transformers)

 #### Readability
- [textstat](https://textstat.org)

 #### Word Count
 - prompt and response wordcount

 #### Number of Sentences
 - number of sentences in prompt and response

 #### Non-alphabetical characters
 - number of non-alphabetical characters in response

 ## Modeling

 We implemented two classification models: Multinomial Logistic Regression and LightGBM (LGBM) for our three-class problem. Both models were trained and evaluated using an 80/20 train/test split, achieving an overall accuracy of 45%.

- **LightGBM Configuration:**
    - **Objective:** Multiclass classification (3 classes)
    - **Parameters:**
        - n_estimators: 500
        - learning_rate: 0.03
        - num_leaves: 64
        - max_depth: 10
        - min_child_samples: 20
        - subsample: 0.8
        - colsample_bytree: 0.8
        - reg_alpha: 0.1
        - reg_lambda: 0.1
        - random_state: 42
    - **Training Details:**
        
        Used callbacks for early stopping (stopping_rounds=30) and periodic evaluation (every 10 rounds), optimizing multi-class log loss during training.
        
- **Multinomial Logistic Regression Configuration:**
    - **Approach:**
        
        Implemented using scikit-learn's LogisticRegression with the multinomial setting.
        
    - **Parameters:**
        - solver: 'lbfgs' (chosen for its memory efficiency and effective optimization with large feature sets)
        - max_iter: 10,000
    - **Label Conversion:**
        
        Converted one-hot encoded labels to integer class labels using the argmax function on the softmax outputs.


 ## Feature Importance