---
title: RAG Design Trade-offs
date: 2024-03-15 12:00:00 -0
categories: [RAG, Design]
tags: []
---

Tools like LlamaIndex have made it easier to create RAG systems. As of now, the main challenge isn't building them, making a basic RAG demo is quite simple. The real difficulty lies in crafting a system that is properly evaluated and that can be deployed at scale, is reliable, secure, and compatible with enterprise solutions.

For a deeper understanding of RAG systems and their components, I recommend reading [this post by Michal Oleszak](https://towardsdatascience.com/designing-rags-dbb9a7c1d729). It provides a detailed overview and explores various design choices. In this text, I'll expand on the performance, cost, compatibility, and solutions trade-offs of each component, focusing solely on LlamaIndex solutions.

## Main RAG components

RAG systems can be decomposed mainly in the next major components:

1. Loading, Processing and Transforming
2. Data Storage and Organization
3. Retrieval
4. Augmentation and Generation
5. Evaluation

## 1. Loading, Processing and Transforming 
The initial step involves consuming the data and making it ready to be used by RAG. This is comparable to data cleaning and feature engineering pipelines in Machine Learning, or ETL pipelines in Data Engineering.

The primary goal at this stage is to retrieve and convert the data into a vector representation. The idea is that similar inputs will yield similar vector representations, whereas different inputs will be positioned far apart in the embedding space. This is crucial for enabling retrieval, allowing us to search for content that is relevant or similar to the user's query.

### 1.1. Loading

Defining the data ingestion process is critical, considering the specific requirements of each use case. LlamaIndex offers a [wide set of utilities through LlamaHub](https://llamahub.ai/?tab=readers), facilitating the easy ingestion of data for search and retrieval by a LLM.

However, it's essential to balance the trade-offs between performance and compatibility among these utilities. For example, utilizing [LLamaParse](https://docs.llamaindex.ai/en/stable/module_guides/loading/connector/llama_parse/) for efficient parsing and representation of files can notably improve retrieval speed and context augmentation. Nevertheless, LLamaParse currently supports only PDF files, underscoring the importance of considering the data format right from the beginning of the design process.

### 1.2. Processing
Indexing new data can occur in either batch mode or streaming mode. The choice between these modes significantly impacts how quickly new data becomes available to the RAG system.

- **Batch:** In this mode, once in a pre-defined timeframe the data is collected and passed to the indexing model. It offers a cost-effectiveness and simpler maintenance solution. Batch mode is suitable for applications that do not require real-time insights, efficiently managing large volumes of data at regular intervals.

- **Streaming:** This mode continually indexes fresh data as soon as it becomes available. It is crucial for scenarios where the chatbot requires the most up-to-date information. Streaming mode enables organizations to derive real-time insights and promptly act on incoming data, making it indispensable for time-sensitive applications.

### 1.3. Transforming
Once the data is loaded, the next step is to transform it. These transformations involve tasks like chunking, metadata extraction, and embedding, all of which are crucial for optimizing performance, cost, and compatibility.

#### 1.3.1. Chunk size

The chunking strategy chosen has a substantial impact on the effectiveness of the RAG output. Various strategies include:
- **Fixed Size Chunking:** Dividing content into fixed-size chunks, such as a set number of words or characters.
- **Chunk Overlapping:** Allow adjacent fragments to share certain common information. This is very useful for improving the context of chunks as allows some continuity between the fragments. 
- **Coherence-based Chunking:** Arranging chunks based on topic changes to ensure each chunk maintains semantic coherence. However, this strategy demands significant computational resources.
- **Structural Chunking:** Designed for tagged file formats like HTML, this strategy requires storing metadata for each chunk, leading to higher storage requirements.

Several factors should be taken into account when making chunking decisions:
- **Document sizes:** The size of the document is crucial, as chunking a tweet differs significantly from chunking a full research paper.
- **Design of the query:** Longer queries indicate a detailed search, suggesting smaller chunk lengths may be preferable as context for specific answers.
- **Coherence of response:** Smaller chunks offer less context but reduce noise, potentially yielding precise but incomplete information retrieval. Larger chunks provide more context but risk overlooking fine-grained details and may contain more noise, leading to outputs with hallucinations.
- **Cost and response time:** Smaller chunks require less processing, resulting in shorter response times. Additionally, considerations should extend to synthesis time and cost, which tend to increase with larger chunk sizes.

Is there a method to empirically determine the optimal chunk size? The [LlamaIndex Response Evaluation](https://www.llamaindex.ai/blog/evaluating-the-ideal-chunk-size-for-a-rag-system-using-llamaindex-6207e5d3fec5) addresses this question. They emphasize that finding the best chunk size for a RAG system involves both intuition and empirical evidence. With this module, you can experiment with different sizes and base your decisions on concrete data, evaluating the efficiency and accuracy of your RAG system using the following criteria:
- **Faithfulness:** Evaluate the absence of 'hallucinations' by comparing the query with the retrieved contexts. Measure whether the response from a query engine matches any source nodes.
- **Relevancy:** Determine if the response effectively answers the query and if it aligns with the source nodes.
- **Response Generation Time:** Larger chunk sizes increase the volume of information processed by the LLM to generate an answer, slowing down the system and affecting its responsiveness.

#### 1.3.2. Embedding
Embedding models accept text as input and generate a vector to capture the text's semantics. This vector representation enables similarity search by mapping both external data and user queries onto the same space. This mathematical relationship facilitates semantic search, enabling users to input a query and locate documents closely related to its meaning.

When selecting an indexing model, it's crucial to weigh trade-offs between performance, cost, compatibility, and privacy:
- **Quality vs Cost:** The chosen indexing model affects the quality of stored embeddings and the relevance of retrieved contexts. Proprietary and larger models typically offer better performance but may come with higher costs.
- **Speed:** Slow indexing models can impact the response time, affecting user experience. Speed is critical, as indexing occurs not only during embedding of new data but also at synthesis time when generating responses.
- **Compatibility and Scalability:** Changing the indexing model requires re-indexing all data, which can be resource-intensive, especially for large datasets. Thus, it's essential to choose a model that balances current needs with future scalability and efficiency.
- **Privacy:** Data privacy concerns, especially in sensitive domains like finance and healthcare, may influence the choice of embedding services.
- **Embedding Types:** Various embedding types address unique challenges and requirements in different domains. Understanding your use case helps in selecting the appropriate embedding type, whether dense embeddings for capturing semantic meaning or sparse embeddings for specific information. Additionally, multi-vector embeddings or variable dimension embeddings offer innovative approaches to address specific needs.

## 2. Data Storage and Organization
Once we have passed our input data to an indexing model to create the embeddings, we need to persist them somewhere from where they can be retrieved when needed.

### 2.1. Vector Databases
In a [previous post](https://bubl-ai.com/posts/Understanding-Vector-Databases/), we broadly covered what Vector Databases are. In essence, Vector Databases are optimized solutions for storing vector representations and conducting efficient searches across them, ideal for production-ready RAG systems. A comparison of different Vector Databases can be found [here](https://www.vecdbs.com/).

When selecting your database, consider the following:
- **Integration:** Ensure compatibility with your development framework.
- **Managed vs Self-Hosted:** Decide based on your team's capabilities and needs. Consider if you have a specialized engineering team for self-hosting and weigh the opportunity cost. Alternatively, managed databases might be a suitable solution.
- **Pricing:** Options range from cloud-based plans where you pay according to your usage, to self-hosted solutions where you pay only for the resources utilized.
- Security and Privacy: Check if the Vector Database complies with SOC, GDPR, HIPAA or any other regulations that applies to you.
- **Performance and Processing:** Assess your app's latency requirements, whether it serves online streaming of data or operates in batch offline mode.
- **Search Support:** Consider if the database offers features like Multi-modal, Full Text Search, Similarity Search, and Hybrid Search.
- **Reliability:** Review uptime commitments in Service Level Agreements.
- **Embedding Compression:** When dealing with large datasets, compression becomes important. Some provide built-in compression, reducing storage usage.
- **Developer Experience:** Evaluate the day-to-day developer experience using the database. Consider factors like open-source availability, API documentation, customization options, error handling, and technical support.
- **Auto-Scaling:** Determine if the database ensures constant availability by adapting to traffic and if scaling up as data size grows is straightforward.

### 2.2. Document Hierarchies
In practical application, RAG systems may face challenges with limited retrieval accuracy particularly when dealing with an extensive set of indistinguishable documents. One effective solution is to implement a document hierarchy, which organizes data in a hierarchical manner to enhance information retrieval. This hierarchy associates chunks with nodes and organizes nodes in parent-child relationships.

**Pros:**
- Improves efficiency and facilitates faster and more reliable data retrieval and processing.
- Allows for a nuanced understanding of context, distinguishing between similar sections with subtle differences.
- Enhances retrieval reliability and reduces hallucination issues related to chunk retrieval.

**Cons:**
- Requires domain-specific knowledge to build relevant hierarchy structures.
- May involve complexity in creating and maintaining the hierarchy over time.


### 2.3. Knowledge Graphs
Graph RAG is an approach to retrieve information from a Knowledge Graph for a given task and serves as a compelling alternative to the traditional "split and embedding" method discussed earlier. In this approach, data is represented as a network consisting of various types of entities (nodes) and their relationships (edges).

Similar to document hierarchies, a Knowledge Graph can consistently and accurately retrieve related rules and concepts, significantly reducing hallucinations. Unlike document hierarchies, Knowledge Graphs map relationships using natural language, enabling even non-technical users to build and modify rules and relationships to manage their enterprise RAG systems.

Therefore, selecting a Knowledge Graph is ideal for organizations requiring a robust tool to structure complex data in an interconnected network. Knowledge Graphs facilitate data representation and trace relationships and lineage between data points. They excel at handling complex, nuanced queries based on connection types, node types, and structure. Moreover, Knowledge Graphs can capture rich semantic relationships that may be overlooked in a vectorized embedded space.

Knowledge Graphs excel in use cases with a complex network of interconnected data points. In such scenarios, the system requires not only information retrieval but also a profound understanding of the relationships and interdependencies among different entities. Knowledge Graphs thrive in this intricate environment by offering a structured representation of relationships between entities.

LlamaIndex offers a range of components that simplify the creation and utilization of Knowledge Graphs:
- `KnowledgeGraphIndex` and `GraphStore`: These components help creating an effective Knowledge Graph from any data source supported by LlamaHub.
- `KnowledgeGraphQueryEngine`: It enables leveraging and querying an existing Knowledge Graph without requiring specific knowledge related to the storage system.
- `KnowledgeGraphRAGRetriever`: This component conducts a search on entities relevant to the question, retrieves the subgraph of those entities, and constructs context based on the subgraph.

You can find an example of how to create and query a Knowledge Graph with LlamaIndex [here](https://docs.llamaindex.ai/en/stable/examples/query_engine/knowledge_graph_query_engine/). Additionally, [this video](https://www.youtube.com/watch?v=bPoNCkjDmco) provides valuable insights by illustrating a real example, helping to understand the differentiation of a Graph RAG from other options and its benefits.

## 3. Retrieval
Retrieval involves identifying pieces of stored data that are relevant to the user's query. At its core, it consists on embedding the query using the same model employed for indexing and then comparing the embedded query with the indexed data. They serve as a crucial component in query engines for retrieving relevant context. Links to LLamaIndex [Query Engine](https://docs.llamaindex.ai/en/stable/module_guides/deploying/query_engine/) and [Chat Engine](https://docs.llamaindex.ai/en/stable/module_guides/deploying/chat_engines/).

The effectiveness of a RAG system depends entirely on the search quality of the backend retrieval system. While LLMs are great at understanding and answering questions, their potential cannot be fully realized if the system lacks high-quality search capabilities for scanning large volumes of information and retrieving the most relevant ones to answer the user query.

Next, we will discuss various retrieval strategies available, covering their advantages and disadvantages. It's worth noting that regardless of the chosen retrieval strategy, it's crucial to consider how many of the retrieved documents will be utilized. This can be achieved by selecting the top-k closest chunks or by establishing a similarity cutoff. 

### 3.1. Retrieval Strategy

#### 3.1.1. Keyword Search
Keyword-based retrieval operates by matching precise words or phrases from a query with those found in documents. This method employs sparse embeddings, also referred to as sparse vector search, where vectors are primarly composed of zero values with only a handful of non-zero values.

Sparse vectors excel in domains and scenarios abundant with rare keywords or specialized terms. This is because they represent each dimension as a word or subword, simplifying the interpretation of document rankings. 

While this method is straightforward and fast, it has its limitations. It can frequently yield low-quality, irrelevant results as it struggles to handle semantic similarity, which can pose challenges with misspellings, synonyms, or polysemy. Additionally, it disregards the context or meaning of words, potentially leading to irrelevant outcomes.

#### 3.1.1. Semantic Search
Semantic search using Deep Learning models has become a very important feature for most search engines, enabling the understanding of query text meanings rather than simply relying on keyword matching. These models produce numerical vector representations of data, typically densely packed with information and mostly comprised of non-zero values.

The key strength of dense vector search lies in its ability to effectively handle semantic similarity. However, its weakness lies in exact matching, where it may struggle to retrieve documents containing the exact query terms but are semantically dissimilar overall.

Another drawback of this approach is latency, which refers to the time taken to retrieve relevant documents. The retriever system needs to compare query embeddings with embeddings of each document indexed in a vector database to identify relevant documents. This process can be time-consuming and may result in high latency, which may be unacceptable for certain real-world scenarios. Possible solutions to mitigate this include Metadata Filtering and the use of Approximate Nearest Neighbors.

#### 3.1.2. Hybrid Search
What if we could leverage the strengths of both Keyword and Semantic Searches simultaneously? Hybrid search offers a solution by combining results from semantic search with those from keyword search. This approach is particularly useful for scenarios where we aim to incorporate semantic search capabilities while still requiring exact phrase matching for specific terms, such as product or patient names.

The process to implement an hybrid search is simple. Keyword search and vector search typically yield separate sets of results, usually in the form of sorted lists based on calculated relevance. To generate a hybrid search, these calculated scores are weighted using a parameter `alpha`, which determines the balance between keyword-based and semantic search. Some mixing methods include Reciprocal Ranked Fusion, [Relative Score Fusion](https://weaviate.io/blog/weaviate-1-20-release), and [Distribution-Based Score Fusion](https://medium.com/plain-simple-software/distribution-based-score-fusion-dbsf-a-new-approach-to-vector-search-ranking-f87c37488b18).

-----
WIP
## 4. Augmentation and Generation
## 5. Evaluation
-----