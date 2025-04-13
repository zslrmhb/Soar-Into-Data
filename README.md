# No L’s in LLMs
> Analyze AI-generated responses by engineering linguistic features, readability, and semantic similarity to predict winning models and improve prompt-response interaction quality.

[DevPost Link](https://devpost.com/software/no-l-s-in-llms#updates)

## features-v3.csv
- prompt_wordcount, resa_wordcount


 ## EDA

 ## Feature Engineering
 ### Features Categories

 #### Sentiment Score
 - [VADERsentiment Analysis](https://github.com/cjhutto/vaderSentiment)
 - Sentiment score ranging [-1, 1] for negative to positive sentiment

 #### Cosine Similarity
 - [SentenceTransfomer](https://huggingface.co/sentence-transformers)

 #### Readability


 #### Word Count
 - prompt and response wordcount

 #### Number of Sentences
 - number of sentences in prompt and response

 #### Non-alphabetical characters
 - number of non-alphabetical characters in response

 ## Modeling


 ## Feature Importance