---
title: Fine-tuning with LlamaIndex
date: 2024-05-04 12:00:00 -0
categories: [Fine-tuning, LlamaIndex]
tags: []
---

Fine-tuning a model involves updating it with new data to enhance its performance. This includes generating better results, reducing hallucinations, including more data holistically, and lowering both latency and costs. Fine-tuning not only improves output quality but can also enhance how the system retrieves information.

This post was inspired by the [LlamaIndex fine-tuning overview](https://docs.llamaindex.ai/en/stable/use_cases/fine_tuning/), I've prepared a [few examples](https://github.com/bubl-ai/llamaindex-project/tree/main/examples/fine-tuning) based on those shared by them. The next examples cover everything that is discussed in this post, and offer valuable insights for anyone interested in gaining a deeper understanding on fine-tuning:
- Embeddings
   - [Embedding with Sentence Transformer](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/fine-tuning/fine-tune_embedding/01_intro_and_naive.ipynb)
   - [Embedding with FineTuneEngine](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/fine-tuning/fine-tune_embedding/02_embedding.ipynb)
   - [2-layer adapter](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/fine-tuning/fine-tune_embedding/03_embedding_adapter.ipynb)
   - [Custom adapter](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/fine-tuning/fine-tune_embedding/04_embedding_custom_adapter.ipynb)
- LLMs
   - [Distill GPT4 to GPT3.5](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/fine-tuning/fine-tune_llm/01_distill_gpt4_to_gpt3.ipynb)
   - Domain Specific Language: *Work in progress.*
   - *Will be including more.*


## Fine-tune LLMs
Open-source LLMs are significant achievements on their own. They perform well on many public benchmarks, often matching or even surpassing proprietary models. This makes them a reliable choice for complex applications, including RAG systems and agents. Fine-tuning these LLMs can be used to further improve them for specific cases, such as:
- **Output Styling:** Enhance the model’s ability to generate text by improving its understanding of syntax, style, and rules.
- **Domain-Specific Language Improvement:** Open-source models sometimes struggle with specific technical languages like SQL or Python. By training with a relevant text-to-code dataset, these models can become more proficient at generating accurate responses in these languages.
- **LLM Distillation:** This process focuses on replicating a larger model's performance for a specific task in a smaller and more cost-effective model. It starts by using the larger model to label data, which then trains a smaller "student" model to perform similarly on that task.
- **Function Calling:** This involves enhancing the model's ability to extract structured data by improving its function-calling abilities.
- **Improving re-ranking:** Develop a re-ranker for specific domains or datasets to optimize the ordering of search results, enhancing the relevance of the top-ranked nodes.
- **LLM-based Evaluators:** Fine-tune evaluators, known as judges, distilled from larger models. These evaluators can achieve high levels of agreement with human judgments, allowing for the creation of specialized evaluators based on these larger models.


## Fine-tune Embeddings
The key question is how to improve retrieval. Often, the existing embeddings might not be ideal for your specific data, which can prevent them from working well with your retrieval objective. Fine-tuning these embeddings is a solution for this problem. To do this, you need training data consisting of text pairs that should be either close together (*positive*) or far apart (*negative*). Tools like the LlamaIndex module’s `MultipleNegativesRankingLoss` can help during training by automatically generating *positive* training pairs from unstructured text, while *negative* pairs are randomly selected from different text chunks. Here are some ways to improve system retrieval through fine-tuning.
- [**Embedding:**](https://medium.com/llamaindex-blog/fine-tuning-embeddings-for-rag-with-synthetic-data-e534409a3971): Develop more meaningful embedding representations to boost retrieval performance by fine-tuning the embedding model over an unstructured text corpus that closely resemble the real data for a specific scenario..
- [**Embedding Adapter:**](https://www.llamaindex.ai/blog/fine-tuning-a-linear-adapter-for-any-embedding-model-8dd0a142d383): A linear adapter applies a transformation to the query embeddings while keeping the document embeddings unchanged. This optimizes the query embeddings for specific data and queries. The advantage is that it works with any model, without the need to re-embed documents.
- [**Routers:**](https://docs.llamaindex.ai/en/stable/examples/finetuning/router/router_finetune/) These are modules that enhance decision-making by selecting the most appropriate data source from multiple options based on a user query.