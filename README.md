# CoviCare - A medical assistant

This is CoviCare, a medical chatbot designed to help covid patients and provide mental support. This chatbot was developed as a part of the DASS Project taken up in the Spring 2024 semester based on client requirements presented to the team.This chatbot is powered by the Gemini model provided by Google. This chatbot has been designed to strictly pertain to the given topic and also trained on additional information about Covid found in Wikipedia.

# System requirements

System : Linux,Windows,Mac\
Required dependencies: 

```
Django
langchain-chroma
langchain-community
langchain-text-splitters
nltk
google
transformers
bert-score
pandas
tqdm
google.generativeai
pandas
tqdm
sentence-transformers
```

Memory : Upto 600-800 MB free space(recommended)

# Codebase

The codebase consists of three major directories:
- ./code
- ./docs
- ./MoM

### ./docs
The ./docs directory contains all relevant documentation required for the project starting from Project plan,Project Status Tracker,Project Scope document,Project SRS document,Project Design Document and Testing Planner. All these documentations contain multiple revisions which were carried out as and when required during the project's journey.

### ./MoM

The ./MoM directory contains the Minutes of Meeting documents recorded in each meeting with the client. These were used to capture key areas of the meet for future reference and serves as a source of trust and absolute judgement in cases of conflicts or confusions between the team and the client.

### ./code

This directory contains the entire codebase of the project.Both the front-end and back-end files are present in this directory.

# Chatbot Architecture

This chatbot has three layers which the user query goes through to generate an appropriate response for the user.The three layers are described as follows : 

## BERTScore based context similarity.

BERTScore is the state-of-the-art evaluation metric for text-generation and comparison tasks. Using BERTScore we can compare the contextual similarity of two sentences. It performs better than the traditional n-gram based approaches because of the following reasons:

- <strong>Contextual Understanding:</strong> Unlike n-gram methods, which rely on shallow lexical matching, BERTScore leverages pre-trained contextual embeddings, capturing the semantics and context of words in a sentence. This allows for a more nuanced understanding of text similarity, considering factors like word order and syntactic structure.
- <strong>Semantic Representation:</strong> BERTScore utilizes embeddings generated by BERT (Bidirectional Encoder Representations from Transformers), a state-of-the-art neural network architecture trained on large corpora of text. These embeddings encode rich semantic information, enabling BERTScore to capture subtle nuances in meaning that may be missed by n-gram methods.
- <strong>Language Understanding:</strong> BERT, being a transformer-based model, excels in understanding natural language by capturing long-range dependencies and contextual information. This makes BERTScore inherently more adept at evaluating text similarity across various linguistic phenomena and contexts.
- <strong>Transfer Learning:</strong> BERTScore benefits from transfer learning, where the model is pre-trained on a large corpus of text and then fine-tuned for specific tasks. This enables it to generalize well to diverse text comparison tasks and datasets, outperforming traditional metrics that lack such pre-training and fine-tuning capabilities.
- <strong>Evaluation Consistency:</strong> BERTScore offers more consistent evaluations across different datasets and domains compared to n-gram-based metrics, which may suffer from issues like sparsity and vocabulary mismatches. By leveraging pre-trained representations, BERTScore provides a more robust and reliable measure of text similarity.

We used a Covid-19 FAQ dataset available on Kaggle to get the most relevant questions users ask about Covid-19. We then use top-k similar questions found in this dataset with the sample question. After experimenting, we set up a threshold of 0.6 BERTScore to classify a question to be relevant to the topic. Here, we used the value of k=3 and we took 500 data points from the dataset out of the available 10,000 instances due to latency management and resource inefficiencies.

Link to dataset: <a href="https://www.kaggle.com/datasets/mohankrishnan02/covid-faqs">Covid-19 FAQ Dataset</a>

Observed results : 
<!-- Add images -->
![alt text](<Screenshot from 2024-04-20 19-41-49.png>)
## Retrieval Augmented Generation from domain knowledge base

Retrieval Augmented Generation(popularly called "RAG") is an LLM-finetuning technique which focuses on providing context to the LLM from a domain-specific knowledge base instead of manually fine-tuning the LLM by adjusting it's weights to respond better to the given prompts.The major advantage that RAG has over traditional-finetuning is that it is computationally inexpensive. Fine-tuning is an extremely expensive procedure especially for larger LLM's such as Gemini variants, Llama, GPT4 which have more than 7 billion parameters. There expected GPU Memory requirements exceed 10GB at the minimum.


We scraped the Wikipedia page of Covid-19 to create a knowledge base about Covid-19 which the LLM can use to support it's responses. This dataset is present in the 'covid.txt' file. Using ChromaDB vector store and Langchain we created a vectorized database. For every prompt that reaches this layer, the vector retriever provided in Chroma DB finds the most similar context to the question. This context along with the user question is provided to Gemini to generate responses.


## Gemini API

The final layer of the chatbot takes in the formatted prompt along with context and provides a relevant response to the user. We use the free version of Gemini here configured according to default conditions.

# Why RAG ? Example cases

<!-- Add images! -->
![alt text](<Screenshot from 2024-04-20 19-49-36.png>)

Earlier it used to respond like this
![alt text](<Screenshot from 2024-04-20 19-58-20.png>)
![alt text](<Screenshot from 2024-04-20 19-58-45.png>)

# Features

- Supports conversation history, the chatbot is aware of the conversation context and uses it to answer ambiguous questions such as "explain the above in detail".
- Stores all conversations recorded for a user and displays it in the sidebar. The user can switch and restart any conversation with the Chatbot just like ChatGPT.
- User sessions, user can login and save their conversations.

# Pipeline to implement a custom chatbot related to domain-specific knowledge.
- Gather two types of dataset, one to filter out context by recording common type of questions asked by users related to that topic. Another database containing text consisting of information related to the domain.
- Change the dataset paths in views.py. For the question and answer dataset it is recommended to store the data in a CSV containing two columns "response" and "context".
- On changing the datasets in the code, the chatbot will automatically adapt to the new topic.

# Softwares used

- Python
- Django Framework
- JavaScript
- Bootstrap CSS
- HTML5
- Langchain
- BERT Transformer
- Google GenAI API

# How to Use ? 
We can start the server by running the following command in ./code directory: 

```
chmod +x code.sh
./code.sh
```

After this, we can enter the chatbot by clicking on the link present in the terminal.You can also directly use the link http://127.0.0.1:8000

