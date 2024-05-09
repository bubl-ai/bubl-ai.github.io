---
title: RAG Design Trade-offs
date: 2024-04-15 12:00:00 -0
categories: [RAG, Design]
tags: []
---

Tools like LlamaIndex have made it easier to create RAG systems. As of now, the main challenge isn't building them, making a basic RAG demo is quite simple. The real difficulty lies in crafting a system that is properly evaluated and that can be deployed at scale, is reliable, secure, and compatible with enterprise solutions.

For a different perspective on RAG systems and their components, I recommend reading [this post by Michal Oleszak](https://towardsdatascience.com/designing-rags-dbb9a7c1d729). It provides a detailed overview and explores various design choices. For writing this post, I followed a similar structure and flow of ideas, but I am expanding and focusing more on the performance, cost, compatibility, and solutions trade-offs of each component, and using solely LlamaIndex solutions.

## Main RAG components

RAG systems can be decomposed mainly in the next major components:

1. Loading, Processing and Transforming
2. Data Storage and Indexing
3. Retrieval
4. Augmentation
5. Generation
6. Evaluation

## 1. Loading, Processing and Transforming 
The initial step involves consuming the data and making it ready to be used by RAG. This is comparable to data cleaning and feature engineering pipelines in Machine Learning, or ETL pipelines in Data Engineering.

The primary goal at this stage is to retrieve and convert the data into a vector representation. The idea is that similar inputs will yield similar vector representations, whereas different inputs will be positioned far apart in the embedding space. This is crucial for enabling retrieval, allowing us to search for content that is relevant or similar to the user's query.

### 1.1. Loading

Defining the data ingestion process is critical and must consider the specific requirements of each use case. LlamaIndex offers a [wide set of utilities through LlamaHub](https://llamahub.ai/?tab=readers), facilitating the easy ingestion of data for search and retrieval by a LLM.

However, it's essential to balance the trade-offs between performance and compatibility among all utilities. For example, utilizing [LLamaParse](https://docs.llamaindex.ai/en/stable/module_guides/loading/connector/llama_parse/) for efficient parsing and representation of files can notably improve retrieval speed and context augmentation. Nevertheless, LLamaParse currently supports only PDF files, underscoring the importance of considering the data format right from the beginning of the design process.

### 1.2. Processing
Indexing new data can occur in either batch mode or streaming mode. The choice between these modes significantly impacts how quickly new data becomes available to the RAG system.

- **Batch:** In this mode, once in a pre-defined timeframe the data is collected and passed to the indexing model. It offers a cost-effective and simpler maintenance solution. Batch mode is suitable for applications that do not require real-time insights, efficiently managing large volumes of data at regular intervals.

- **Streaming:** This mode continually indexes fresh data as soon as it becomes available. It is crucial for scenarios where the chatbot requires the most up-to-date information. Streaming mode enables organizations to derive real-time insights and promptly act on incoming data, making it indispensable for time-sensitive applications.

### 1.3. Transforming
Once the data is loaded and processed, the next step is to transform it. These transformations involve tasks like chunking, metadata extraction, and embedding, all of which are crucial for optimizing performance, cost, and compatibility.

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
- **Faithfulness:** Evaluate the absence of 'hallucinations' by comparing the retrieved documents with the response.
- **Relevancy:** Determine if the response effectively answers the query and if it aligns with the source nodes.
- **Response Generation Time:** Larger chunk sizes increase the volume of information processed by the LLM to generate an answer, slowing down the system and affecting its responsiveness.

Evaluation processes and metrics will be covered in more detail in the **Evaluation** section of this post.

#### 1.3.2. Metadata
Metadata plays a crucial role in enhancing the context of each chunk within a system. By injecting metadata into chunks, developers can provide additional information that aids in the retrieval process. Examples of metadata include page numbers, titles, summaries of chunks and adjacent chunks, as well as questions that can be answered based on the content.

In many ways, the use of metadata during the retrieval of relevant context can be likened to the utilization of the WHERE clause in a SQL statement. Just as the WHERE clause filters and selects specific data from a database table, metadata helps refine and narrow down the search space, making the retrieval process more efficient and accurate.

Moreover, metadata seamlessly integrates with Vector Databases, enriching the embeddings used in the system. This integration enhances the representation of data and improves the effectiveness of retrieval algorithms. In the subsequent sections, we will delve deeper into the utilization of metadata and its impact on system performance.

For examples and usage of Metadata in LLamaIndex, use [this guide](https://docs.llamaindex.ai/en/stable/module_guides/indexing/metadata_extraction/).

#### 1.3.3. Embedding
Embedding models accept text as input and generate a vector to capture the text's semantics. This vector representation enables similarity search by mapping both external data and user queries onto the same space. This mathematical relationship facilitates semantic search, enabling users to input a query and locate documents closely related to its meaning.

When selecting an indexing model, it's crucial to weigh trade-offs between performance, cost, compatibility, and privacy:
- **Quality vs Cost:** The chosen indexing model affects the quality of stored embeddings and the relevance of retrieved contexts. Proprietary and larger models typically offer better performance but may come with higher costs.
- **Speed:** Slow indexing models can impact the response time, affecting user experience. Speed is critical, as indexing occurs not only during embedding of new data but also during the embedding of a user query.
- **Compatibility and Scalability:** Changing the indexing model requires re-indexing all data, which can be resource-intensive, especially for large datasets. Thus, it's essential to choose a model that balances current needs with future scalability and efficiency.
- **Privacy:** Data privacy concerns, especially in sensitive domains like finance and healthcare, may influence the choice of embedding services.
- **Embedding Types:** Various embedding types address unique challenges and requirements in different domains. Understanding your use case helps in selecting the appropriate embedding type, whether dense embeddings for capturing semantic meaning or sparse embeddings for specific information. Additionally, multi-vector embeddings or variable dimension embeddings offer innovative approaches to address specific needs.

## 2. Data Storage and Indexing
After embedding our input data, the next step is to index and store it in a database that allows for efficient querying to retrieve the closest matching database records. This ensures that the data is readily accessible when needed.

### 2.1. Vector Databases
In a [previous post](https://bubl-ai.com/posts/Understanding-Vector-Databases/), we broadly covered what Vector Databases are. In essence, Vector Databases are optimized solutions for storing vector representations and conducting efficient searches across them, ideal for production-ready RAG systems. A comparison of different Vector Databases can be found [here](https://www.vecdbs.com/).

When selecting your database, consider the following:
- **Integration:** Ensure compatibility with your development framework.
- **Managed vs Self-Hosted:** Decide based on your team's capabilities and needs. Consider if you have a specialized engineering team for self-hosting and weigh the opportunity cost. Alternatively, managed databases might be a suitable solution.
- **Pricing:** Options range from cloud-based plans where you pay according to your usage, to self-hosted solutions where you pay only for the resources utilized.
- **Security and Privacy:** Check if the Vector Database complies with SOC, GDPR, HIPAA or any other regulations that applies to you.
- **Performance and Processing:** Assess your app's latency requirements, whether it serves online streaming of data or operates in batch offline mode.
- **Search Support:** Consider if the database offers features like Multi-modal, Full Text Search, Similarity Search, and Hybrid Search.
- **Reliability:** Review uptime commitments in Service Level Agreements.
- **Embedding Compression:** When dealing with large datasets, compression becomes important. Some provide built-in compression, reducing storage usage.
- **Developer Experience:** Evaluate the day-to-day developer experience using the database. Consider factors like open-source availability, API documentation, customization options, error handling, and technical support.
- **Auto-Scaling:** Determine if the database ensures constant availability by adapting to traffic and if scaling up as data size grows is straightforward.

### 2.2. Indexing
[This LlamaIndex Indexing components guide](https://docs.llamaindex.ai/en/stable/module_guides/indexing/index_guide/) offers a good description of the different types of indexes that are offered in LlamaIndex.

#### 2.2.1. Summary Index
The most basic form of indexing is the Summary Index, which stores all nodes as a sequential chain. When querying, the index scans through this list of nodes and, using an embedding-based query or a keyword filter, retrieves the top-k neighbors.

#### 2.2.2. Tree Index
RAG systems might encounter difficulties with limited retrieval accuracy, especially when handling a large set of indistinguishable documents. One effective solution is to deploy a tree index, which organizes data hierarchically to improve information retrieval. In this structure, each node in the tree summarizes the documents that are its children. This hierarchical arrangement establishes associations between chunks and nodes, organizing nodes in parent-child relationships.

During query time, the process involves initial query processing to identify keywords relevant to the query. Subsequently, the tree is traversed from root nodes to leaf nodes:
1. The index checks if the keywords in the query are present in the node’s summary.
2. If present, the index descends into the node’s children.
3. If not present, the index moves on to the next node.
The index determines how many child nodes it can select given a parent node based on chosen parameters.

**Pros:**
- Improves efficiency and facilitates faster, scalable and more reliable data retrieval and processing. 
- Allows for a nuanced understanding of context, distinguishing between similar sections with subtle differences.
- May enhance retrieval reliability and reduces hallucination issues related to chunk retrieval.

**Cons:**
- Requires domain-specific knowledge to build relevant hierarchy structures.
- May involve complexity in creating and maintaining the hierarchy over time.

#### 2.2.3. Vector Store Index
The vector store index is the most common index type. As discussed in Section 2.1., each Node and its corresponding embedding are stored in a Vector Store as vectors. These vectors represent the meaning of the text and facilitate finding relevant documents based on the user’s query.

During query time, the process begins with the query being processed by the same embedding model to generate a vector store. Subsequently, the vector of the query is compared to all the documents present in the index, and the top-k most similar nodes are retrieved.

#### 2.2.4. Keyword Table
The keyword table index operates by extracting keywords from each Node and constructing a mapping from each keyword to the corresponding Nodes containing that keyword.

During query time, relevant keywords are extracted from the query, which are then matched with pre-extracted Node keywords to fetch the corresponding Nodes. Subsequently, the extracted Nodes are used for synthesis.

#### 2.2.5. Knowledge Graphs
Graph RAG is an approach to retrieve information from a Knowledge Graph for a given task and serves as a compelling alternative to the split-embed method used in previous index types. In this approach, data is represented as a network consisting of various types of entities (nodes) and their relationships (edges).

Similar to document hierarchies, a Knowledge Graph can consistently and accurately retrieve related rules and concepts, significantly reducing hallucinations. Unlike document hierarchies, Knowledge Graphs map relationships using natural language, enabling even non-technical users to build and modify rules and relationships to manage their enterprise RAG systems.

Therefore, selecting a Knowledge Graph is ideal for organizations requiring a robust tool to structure complex data in an interconnected network. Knowledge Graphs facilitate data representation and trace relationships and lineage between data points. They excel at handling complex, nuanced queries based on connection types, node types, and structure. Moreover, Knowledge Graphs can capture rich semantic relationships that may be overlooked in a vectorized embedded space.

Knowledge Graphs excel in use cases with a complex network of interconnected data points. In such scenarios, the system requires not only information retrieval but also a profound understanding of the relationships and interdependencies among different entities. Knowledge Graphs thrive in this intricate environment by offering a structured representation of relationships between entities.

LlamaIndex offers a range of components that simplify the creation and utilization of Knowledge Graphs:
- `KnowledgeGraphIndex` and `GraphStore`: These components help creating an effective Knowledge Graph from any data source supported by LlamaHub.
- `KnowledgeGraphQueryEngine`: It enables leveraging and querying an existing Knowledge Graph without requiring specific knowledge related to the storage system.
- `KnowledgeGraphRAGRetriever`: This component conducts a search on entities relevant to the question, retrieves the subgraph of those entities, and constructs context based on the subgraph.

You can find an example of how to create and query a Knowledge Graph with LlamaIndex [here](https://docs.llamaindex.ai/en/stable/examples/query_engine/knowledge_graph_query_engine/). Additionally, [this video](https://www.youtube.com/watch?v=bPoNCkjDmco) provides valuable insights by illustrating a real example, helping to understand the differentiation of a Graph RAG from other options and its benefits.

## 3. Retrieval
Retrieval involves identifying pieces of stored data that are relevant to the user's query. At its core, it consists on embedding the query using the same model employed for indexing and then comparing the embedded query with the indexed data. Links to LLamaIndex [Query Engine](https://docs.llamaindex.ai/en/stable/module_guides/deploying/query_engine/) and [Chat Engine](https://docs.llamaindex.ai/en/stable/module_guides/deploying/chat_engines/).

The effectiveness of a RAG system depends entirely on the search quality of the backend retrieval system. While LLMs are great at understanding and answering questions, their potential cannot be fully realized if the system lacks high-quality search capabilities for scanning large volumes of information and retrieving the most relevant ones to answer the user query.

Next, we will discuss various retrieval strategies available, covering their advantages and disadvantages. It's worth noting that regardless of the chosen retrieval strategy, it's crucial to consider how many of the retrieved documents will be utilized. This can be achieved by selecting the top-k closest chunks or by establishing a similarity cutoff. 

### 3.1. Search

#### 3.1.1. Keyword Search
Keyword-based retrieval operates by matching precise words or phrases from a query with those found in documents. This method employs sparse embeddings, also referred to as sparse vector search, where vectors are primarily composed of zero values with only a handful of non-zero values.

Sparse vectors excel in domains and scenarios abundant with rare keywords or specialized terms. This is because they represent each dimension as a word, simplifying the interpretation of document rankings. 

While this method is straightforward and fast, it has its limitations. It can frequently yield low-quality, irrelevant results as it struggles to handle semantic similarity, which can pose challenges with misspellings, synonyms, or polysemy. Additionally, it disregards the context or meaning of words, potentially leading to irrelevant outcomes.

#### 3.1.1. Semantic Search
Semantic search using Deep Learning models has become a very important feature for most search engines, enabling the understanding of query text meanings rather than simply relying on keyword matching. These models produce numerical vector representations of data, typically densely packed with information and mostly comprised of non-zero values.

The key strength of dense vector search lies in its ability to effectively handle semantic similarity. However, its weakness lies in exact matching, where it may struggle to retrieve documents containing the exact query terms but are semantically dissimilar overall.

Another drawback of this approach is latency, which refers to the time taken to retrieve relevant documents. The retriever system needs to compare query embeddings with embeddings of each document indexed in a vector database to identify relevant documents. This process can be time-consuming and may result in high latency, which may be unacceptable for certain real-world scenarios. Possible solutions to mitigate this include Metadata Filtering and the use of Approximate Nearest Neighbors.

#### 3.1.2. Hybrid Search
What if we could leverage the strengths of both Keyword and Semantic Searches simultaneously? Hybrid search offers a solution by combining results from semantic search with those from keyword search. This approach is particularly useful for scenarios where we aim to incorporate semantic search capabilities while still requiring exact phrase matching for specific terms, such as product or patient names.

The process to implement an hybrid search is simple. Keyword search and vector search typically yield separate sets of results, usually in the form of sorted lists based on calculated relevance. To generate a hybrid search, these calculated scores are weighted using a parameter *alpha*, which determines the balance between keyword-based and semantic search. Some mixing methods include Reciprocal Ranked Fusion, [Relative Score Fusion](https://weaviate.io/blog/weaviate-1-20-release), and [Distribution-Based Score Fusion](https://medium.com/plain-simple-software/distribution-based-score-fusion-dbsf-a-new-approach-to-vector-search-ranking-f87c37488b18).

### 3.2. Advanced Retrieval Strategies
There are a variety of [advanced strategies](https://docs.llamaindex.ai/en/stable/optimizing/advanced_retrieval/advanced_retrieval/) available in LlamaIndex to improve the information retrieval.
- [**Re-ranking:**](https://pub.towardsai.net/advanced-rag-04-re-ranking-85f6ae8170b1) This strategy involves the reordering of retrieved documents based on their relevance to the query. Utilizing another model, such as another LLM, both the query and document are fed into the model, which then produces a single score indicating the document's relevance to the given query.
- [**Recursive:**](https://docs.llamaindex.ai/en/stable/examples/query_engine/pdf_tables/recursive_retriever/) This approach not only retrieves the most directly relevant nodes but also explores node relationships by applying further queries to the document. If there are references to other information within a chunk that may helpful in answering the query, it retrieves that additional information. For instance, if a chunk contains a summary about a table and also includes the command to load it, both the summary and the queried table would be retrieved.
- [**Small-to-Big:**](https://towardsdatascience.com/advanced-rag-01-small-to-big-retrieval-172181b396d4) A simple approach is to embed a big text for retrieval, if retrieved this same text is used for synthesis. This often leads to suboptimal results due to filler text obscuring the semantic representation, a phenomenon known as "lost in the middle." The Small-to-Big retrieval strategy addresses this by using smaller text chunks (can be sentences) during retrieval and then providing the larger text chunk to which the retrieved text belongs to the LLM for synthesis. This helps avoid "lost in the middle" scenarios, potentially improving retrieval performance significantly.

You can find examples of re-rankers and other node post-processing modules within LlamaIndex, [here](https://docs.llamaindex.ai/en/stable/module_guides/querying/node_postprocessors/node_postprocessors/?h=coherererank#coherererank).

## 4. Augmentation
When discussing augmentation, it's important to differentiate between two types: Query and Prompt Augmentation. While there may be significant overlap between these two, and some methodologies described below may be applicable to both, it's crucial to emphasize that augmentation can occur at different stages of the pipeline, which must be considered when designing RAG systems.

- **Query augmentation:** This occurs before retrieval and involves modifying the user's original query. The main objective is to enhance retrieval.
- **Prompt Augmentation:** This occurs after retrieval and involves modifying the prompt that will be sent to the LLM to generate the final output. The main objective is to improve the generation of the final output with a given knowledge/context.

### 4.1. Query Augmentation
Query augmentation addresses the issue of how a query is phrased and whether it is optimal for finding relevant data to support the answer. Techniques for query transformation significantly enhance RAG performance and higher quality retrieved results, especially when dealing with short and vague user queries.

Here is a list of different query transformations:
- **Routing:** Retain the query but determine the relevant subset of pipelines that the query applies to.
- **Query rewriting:** Maintain the same pipelines but rephrase the query using an LLM to avoid grammatical issues.
- **Sub-query generation:** Divide the query into smaller, more detailed sub-queries based on the original query and pass them to different pipelines.
- **ReAct Agent Tool Picking:** Identify the tool to pick and the query to execute on the tool based on the initial query.
- **Hypothetical Document Embeddings:** Create a hypothetical document for an incoming query using an LLM, embed the document, and use it together with the original query to retrieve similar documents. This method assumes that the generated document will share some similarities with a good answer and may be closer in the embedding space than the query.
- [**QueryFusionRetriever:**](https://plg.uwaterloo.ca/~gvcormac/cormacksigir09-rrf.pdf) This method offers an efficient way to re-rank retrieved nodes without excessive computation or dependence on external models. It generates similar queries to the user query, retrieves and re-ranks the top n nodes from each generated query using the Reciprocal Re-rank Fusion algorithm.

Please refer to [this LlamaIndex example](https://docs.llamaindex.ai/en/stable/examples/query_transformations/query_transform_cookbook/), which provides more detailed information on different query transformation methods.It's crucial to be cautious when using these methods, as augmentation may be open-ended and could potentially mislead or misinterpret the context, leading to dangerous or unsafe generated outputs.

### 4.2. Prompt Augmentation
The system prompt is the message added to the context and query sent to the LLM. The precise wording of this prompt is crucial for RAG performance. In a general sense, you can pre-program your system to behave as desired. For example, explicitly mentioning that only information from the provided context should be used, not general knowledge. Additionally, it can also serve as a template for how the response should appear.

There are various fascinating techniques for prompt engineering. You can find a well-described list of advanced prompt engineering techniques [here](https://www.promptingguide.ai/techniques). Below are some examples:

- **Few-Shot:** Support the model with a few input-output examples to enable in-context learning, steering the model to better performance.
- **Chain-of-Thought:** Structuring the input prompt to mimic human reasoning, aiding LLMs in tasks requiring logic, calculation, and decision-making.
- **Context Transformation:** Dynamically adding context transformations as functions in the prompt variable. For instance, using Personal Identifiable Information masking in LlamaIndex for legal compliance and data privacy/security concerns.
- **ReAct:** Reasoning and Acting with LLMs, enabling the model to generate verbal reasoning traces and actions for a task.

Other very interesting prompt engineering techniques include Generate Knowledge, Tree of Thoughts, Thread of Thoughts, and Emotion Prompt. This is a big topic so I want to give it a bigger space, stay tuned for future posts covering this topic in more detail!

## 5. Generation
At this stage we need to ask, which model should be used to generate a response for our prompt? With LlamaIndex, it's simple to change the underlying LLM and plug in any model listed on their [LLM API Reference documentation](https://docs.llamaindex.ai/en/stable/module_guides/models/llms/modules/) or [Langchain’s LLM page](https://python.langchain.com/docs/integrations/llms/). Moreover, customizing your selected model with the [LlamaIndex Abstractions](https://docs.llamaindex.ai/en/stable/module_guides/models/llms/usage_custom/) is easy, allowing you to control various aspects of the underlying model such as context window size, maximum response size, model output randomness, and vocabulary filtering.

To compare different models, resources like [Chatbot Arena](https://chat.lmsys.org/) or the [HuggingFace Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) can be utilized. When selecting the model, it's essential to consider factors such as:
- **License:** Understand the user requirements and potential pitfalls of the License of the models under consideration and select the one that best suits specific needs.
- **Knowledge Cutoff:** Each LLM is often associated with a claimed knowledge cutoff date or the dates at which the training data was gathered. This information is crucial for applications requiring up-to-date information.
- **Quality and Cost:** The chosen model affects the coherence, grammatical correctness, helpfulness, bias, and safety of the responses, as well as the cost of the solution if a proprietary model is chosen. If budget and latency are not constraints, it's advisable to use the best LLM available for the specific task.
- **Domain-specific Performance:** Identify the strengths or weaknesses of a model using different benchmark metrics. Detailed descriptions of benchmarks can be found in [this post](https://towardsdatascience.com/evaluating-large-language-models-a145b801dce0). Some relevant metrics include ROUGE for text summarization and translation, BLEU for translation. Domain-specific metric descriptions related to math, logic, reasoning, and coding, such as MMLU and AI2 Reasoning Challenge, are available on the About section this [HuggingFace page](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard).

## 6. Evaluation
Reliably evaluating your changes and improvements is a crucial part of LLM application development. Every strategy has pros and cons, and a key aspect of building and improving your application is evaluating whether your changes have enhanced its usefulness, accuracy, performance, and cost.

When developing your LLM application, establishing an end-to-end evaluation workflow can provide valuable guidance. This approach evaluates the quality of your final response given the inputs, and allows you to define a comprehensive evaluation process that encompasses the entire system. Beginning with a small but diverse set of queries can help you gain initial insights into the system's behavior. As you encounter problematic queries or interactions, you can gradually expand your set of examples and automate the evaluation process using a defined set of summary metrics.

[End-to-End evaluation](https://docs.llamaindex.ai/en/stable/optimizing/evaluation/e2e_evaluation/) serves as the holistic view of the system's performance, helping you understand how well it functions in real-world scenarios. With a complex pipeline, it can be challenging to identify which components are influencing your results, as you transition from end-to-end evaluation to [Component Wise evaluation](https://docs.llamaindex.ai/en/stable/optimizing/evaluation/component_wise_evaluation/), conducting sensitivity tests can be beneficial. These tests enable you to identify the most problematic components or data points, allowing you to focus on testing and improving them individually. This approach enhances the overall robustness and effectiveness of your LLM application.

### 6.1. Test Data
Because developers don’t quite know outputs may be directly related to inputs, they need to define an evaluation dataset that’s reflective of their production use cases, and evaluate their system over this dataset using a set of evaluation metrics.

Having a realistic test data is one of the hardest parts of a RAG system. There are solutions that let you easily generate questions that could be answered by the text. This is very useful specially for questions that have clear answers in individual chunks of text. Nevertheless, that is not usually the case in real world scenarios, and in such cases your system may not do a good job answering more meta-level questions or queries that require a high level of reasoning, handle complex dn nuanced queries with a complex interconnected source nodes.

Where can we get data for evaluation purposes?
- **Real World Data:** Ideally, the test data should come from the target users of our RAG system, for example, collected through an early access release. It is important to work closely with the target end users of the system being developed to ensure a clear understanding of the types of questions and inputs they would have. Based on this data, create a good dataset for evaluation purposes that closely resembles what may happen once the solution is deployed.
- **Public Datasets:** When it's not possible to collect data from target users, or if we want to evaluate the RAG before releasing it, we can use one of the LLama Datasets for that purpose. However, this evaluation will not fully represent the production environment our RAG will operate in. [LlamaIndex Datasets](https://www.llamaindex.ai/blog/introducing-llama-datasets-aadb9994ad9e) are a set of community-contributed datasets that enable users to easily benchmark their RAG pipelines for various use cases. Each dataset comprises both question-answer pairs and source context.

### 6.2. Performance Metrics
LlamaIndex offers a wide range of modules to evaluate both Retrieval and Responses, accessible [here](https://docs.llamaindex.ai/en/stable/module_guides/evaluating/modules/). 

#### 6.2.1. Response Evaluation
Let's briefly compare some of the options for Response evaluation:

- [**Faithfulness:**](https://ts.llamaindex.ai/modules/evaluation/modules/faithfulness) This metric assesses whether the response matches the retrieved context. It measures if the generated answer is faithful to the retrieved contexts, indicating whether it was potentially hallucinated. Scores range between 0 and 1, where 1 indicates perfect alignment with the context.
- [**Answer Relevancy:**](https://docs.llamaindex.ai/en/stable/api_reference/evaluation/answer_relevancy/) This evaluates whether the response matches or is relevant to the query. It measures if the response actually answers the query. Scores range between 0 and 1, with 1 indicating high relevance.
- [**Context Relevancy:**](https://docs.llamaindex.ai/en/stable/api_reference/evaluation/context_relevancy/) This metric determines whether the retrieved context is relevant to the query. Similar to Answer Relevancy, it assesses if the retrieved context is pertinent to the query, either generally or for specific source nodes.
- [**Correctness:**](https://docs.llamaindex.ai/en/stable/api_reference/evaluation/correctness/) This assesses whether the answer matches the reference answer. It evaluates the relevance and correctness of a generated answer against a reference answer, providing a score between 0 and 5, where 5 indicates correctness.
- [**Semantic Similarity:**](https://docs.llamaindex.ai/en/stable/examples/evaluation/semantic_similarity_eval/) This metric gauges whether the predicted answer is semantically similar to the ground truth answer.
- [**Guideline Adherence:**](https://docs.llamaindex.ai/en/stable/examples/evaluation/guideline_eval/) It evaluates whether the predicted answer adheres to specific user guidelines.
- [**Toxicity and Bias:**] It is critical to implement responsible safeguards avoiding responses that are rude, disrespectful. The Allen Institute for AI provides [this dataset](https://allenai.org/data/real-toxicity-prompts) that can be used for addressing the risk of generating harmful responses.
- [**Metrics Ensembling:**] Use an ensemble of weaker signals to predict the output of a more expensive evaluation methods with the objective of avoiding expensive evaluations related to LLM calls.

#### 6.2.2. Retrieval Evaluation
[**Retrieval Evaluation**](https://docs.llamaindex.ai/en/stable/examples/evaluation/retrieval/retriever_eval/) addresses the question of whether the retrieved sources are relevant to the query. It compares what was retrieved for the query to a set of source nodes that were expected to be retrieved. Given a dataset of questions and ground-truth rankings, for any given question, the quality of retrieved results is compared to the ground-truth context. Retrievers can be evaluated using ranking metrics such as:
- **Hit Rate:** This metric evaluates how often the system provides the correct answer within the top few guesses. It involves determining the fraction of queries where the correct answer is found within the top-k retrieved documents.
- [**Mean Reciprocal Rank (MRR):**](https://en.wikipedia.org/wiki/Mean_reciprocal_rank) MRR is a measure for evaluating any process that produces a list of possible responses to a sample of queries, ordered by the probability of correctness. It assesses the rank of the highest-placed relevant document for each query and calculates the average of the reciprocals of these ranks across all queries.
- [**Precision:**](https://docs.ragas.io/en/stable/concepts/metrics/context_precision.html) It assesses whether all relevant items present in the contexts are ranked higher. Ideally, all relevant chunks should appear at the top ranks, with scores ranging between 0 and 1, higher scores indicating better precision. Precision measures the fraction of the retrieved documents that are relevant to the query. Low precision is related to hallucination.
- [**Recall:**](https://docs.ragas.io/en/stable/concepts/metrics/context_recall.html) This measures how well the retrieved context aligns with the annotated answer, treated as the ground truth. Recall measures the fraction of the relevant documents that were retrieved by the system. Low recall is associated to responses with lack of context.


#### 6.2.3. LlamaIndex Integrations
LlamaIndex offers seamless integration with various monitoring and evaluation tools and frameworks:

- [**UpTrain:**](https://docs.llamaindex.ai/en/stable/examples/evaluation/UpTrain/) This open-source unified platform provides grades for over 20 preconfigured evaluations, covering language, code, and embedding use cases. It also performs root cause analysis on failure cases and offers insights on how to resolve them. You can find the UpTrain GitHub repository [here](https://github.com/uptrain-ai/uptrain).
- [**Tonic Validate:**](https://docs.llamaindex.ai/en/stable/examples/evaluation/TonicValidateEvaluators/) Tonic Validate includes an open-source SDK and a web UI. The SDK comprises all the necessary tools for evaluating your RAG system, while the web UI serves as a visualization layer on top of the SDK, providing a better understanding of your system's performance beyond raw numbers.
- [**DeepEval:**](https://docs.llamaindex.ai/en/stable/examples/evaluation/Deepeval/) This open-source LLM evaluation framework, available on [GitHub](https://github.com/confident-ai/deepeval), incorporates the latest research to assess LLM outputs. It includes metrics such as G-Eval, Summarization, Answer Relevancy, Faithfulness, Contextual Recall, Contextual Precision, RAGAS, Hallucination, Toxicity, and Bias.
- [**Ragas:**](https://docs.ragas.io/en/stable/index.html#) Ragas facilitates the continuous improvement of LLM and RAG applications through Metrics-Driven Development (MDD). This approach relies on data to make informed decisions, continuously monitoring essential metrics over time to gain valuable insights into application performance.


### 6.3. Cost Analysis
While comprehensive documentation on this topic seems to be currently unavailable, it's crucial to address the cost considerations associated with a RAG system. Essentially, the cost of a RAG system can be broken down into the following components:

- **LLM Calls:** Each interaction with an LLM incurs a certain cost. This cost varies based on factors such as the specific LLM used, the data structure employed, and the parameters utilized during indexing, querying and evaluation.
- **Vector Store:** Whether utilizing a cloud-based or self-hosted vector store, there are costs associated with usage or resources. These costs can vary depending on factors such as storage capacity and data transfer.
- **Opportunity Cost:** This aspect is often overlooked but holds significant importance. Every decision made during the design and implementation of your system represents a trade-off, resulting in a potential loss of gains from alternative choices not pursued. Considering the opportunity cost allows for adjustments to be made along the way, ensuring that resources—whether monetary, labor-related, or computational—are utilized as efficiently as possible.

## Conclusion
The design, development, and implementation of RAG systems pose significant challenges, requiring careful consideration of vast interconnected components. Each component plays a crucial role in determining the system's overall functionality, effectiveness, and efficiency. However, the complexity of these systems is compounded by the various components involved in data management, retrieval, augmentation, and generation.

Therefore, it is crucial to reliably evaluate both the end-to-end solution and each critical component individually. This evaluation process allows for the identification of strengths, weaknesses, and areas for improvement within the system. By assessing the impact of changes on factors such as usefulness, accuracy, performance, and cost, developers can make informed decisions to iteratively enhance the overall quality and impact of the system. To improve your RAG system effectively there are countless of things that you can change, it's advisable to start with solutions that are simple in terms of cost and implementation but promise high expected returns. From there, you can progressively explore more complex enhancements to further improve your system.