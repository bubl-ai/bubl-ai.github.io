---
title: Understanding Vector Databases
date: 2024-03-14 12:00:00 -0
categories: [RAG, Database]
tags: [RAG, Vector-Database]
---

LLMs can become outdated due to infrequent updates, relying on training data that might be several years old. When seeking information not publicly available or absent from the specific model's training data, a solution is needed.

Vector Databases gain significance by allowing you to use your own data, regardless of size, to enhance these models and make them more pertinent to your inquiries. These databases are essential for managing unstructured and semi-structured data, offering capabilities like indexing, distance metrics, and similarity search, making them a crucial tool in the machine learning and AI landscape.

It's important to start by distinguishing between Vector Databases and Vector Indices. The former suits robust, production-grade solutions, addressing scalability, integration, and security. The latter is more fitting for rapid testing but may fall short of meeting the comprehensive needs of production environments.[^fn-nth-1]


## What are Vector Databases?
A Vector Database comprises data stored as mathematical representations, enabling AI models to retrieve past inputs through similarity metrics rather than exact matches, making it possible for a model to understand data contextually. In other words, Vector Databases empower AI models to make comparisons, recognize relationships, and understand context.

One key advantage that positions Vector Databases as an optimal solution for LLMs is their optimization for efficiently storing and processing extensive amounts of vector dat, surpassing the capabilities of traditional databases.[^fn-nth-2]

## How Vector Databases are used in RAG?

A very brief description on how RAG use Vector Databases is the following. Models, often Deep Learning ones, can be used to generate an embedding for each document within the domain of interest, storing it in a Vector Database. Embeddings can be seen as a tool to aggregate your whole set of documents into more manageable chunks that can be fed into the limited context of a LLM. A comparable embedding is computed for a given prompt, and the database is queried to fetch the most similar documents. Finally, the LLM processes this aggregated context (prompt plus relevant documents) to formulate its response.

Vector Databases offer a suite of powerful features designed for efficient data management:

1. **Mathematical Operations and Specialized Indexing**:
    - Use of clustering techniques for filtering and locating similar vectors.
    - Faster and deterministic retrieval of data using specialized Vector indexes.
2. **Efficient and responsive Vector Storage**:
    - Store vectors efficiently using methods like compression.
    - Scale out seamlessly by distributing data across multiple machines, allowing to handle extensive datasets.
    - Enable real-time updates by eliminating the need for complete re-indexing.
3. **Comprehensive Data Management and Streamlined Operations**:
    - Provide comprehensive data management functions, including insertion, deletion, and updating operations.
    - Enhancing overall efficiency by streamlining routine operations such as backups and the creation of data.
4. **Seamless Integration and Enhanced Security**:
    - Seamlessly integrate with broader data processing ecosystems, ensuring compatibility.
    - Enhance data security and access control, safeguarding information within the database.


## Why can't I just use a well known Relational Database?

These databases aren't substitutes; it's probable that your project will involve both Relational and Vector Databases.

Relational databases organize data neatly into tables with rows and columns, perfect for structured data and clear relationships. On the other hand, Vector Databases are designed for storing and fetching vectors efficiently. This makes them excellent for quickly finding similar items and performing analytics in applications that deal with complex data relationships.

## What Vector Database to use?

This question is not straightforward to answer as it may depend on your specific needs. The next image gives a very intuitive landscape of some of the most popular Vector Databases.

![RAG](/images/Vector_Databases/landscape.jpg)_[Source:](https://blog.det.life/why-you-shouldnt-invest-in-vector-databases-c0cd3f59d23c?gi=48719dcb8156c): Vector Database Landscape_


For experimentation purposes I will be testing some of these options. I will make a future post on this, stay tuned!


## References
[^fn-nth-1]: https://www.linkedin.com/pulse/vector-index-vs-database-understanding-key-ankit-khare-tj5lc
[^fn-nth-2]: https://lunabrain.com/blog/riding-the-ai-wave-with-vector-databases-how-they-work-and-why-vcs-love-them/