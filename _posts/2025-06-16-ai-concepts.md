---
layout: post
title: "Modern AI Concepts"
date: 2025-06-15 23:55 -0500
author: nooneswarup
categories: [AI]
tags: [AI, ML]
---

Artificial Intelligence: concepts, tools for working with complex data. 
Here is a breakdown of key components and strategies in the modern AI landscape.

### 1. Data Modalities
AI models can process various types of data, often referred to as modalities.
* **Text:** Documents (`PDFs`, `DOCX`), structured files (`CSVs`), `DataFrames`, and API responses, Dataframes
* **Vision:** Images and videos.
* **Speech:** Audio files.
* **Multi-modal:** A combination of any of the above.

### 2. Understanding Unstructured Data
A significant portion of real-world data lacks a predefined model or organization.
* Approximately **80%** of real-world data is unstructured.
* Since computers primarily understand numbers, we must convert this data. The typical workflow is:
    1.  Use **document loaders** to ingest the data.
    2.  **Chunk** the data into smaller, manageable pieces.
    3.  Generate **embeddings** to convert the chunks into numerical vectors.
    4.  Store these vectors in a **Vector Database** (or use `NumPy` for smaller tasks).
* **Process Flow:**
    * **Indexing:** `Data` → `Embedding Model` → `Vector` → `Vector DB`
    * **Retrieval:** `Query` → `Embedding` → `Similarity Search` → `Retrieved Answer`

### 3. Preprocessing Steps
Before vectorization, raw text data is cleaned and standardized using libraries like `NLTK` and `spaCy`.
* **Text Normalization:** Standardizing text (e.g., converting to lowercase).
* **Part-of-Speech (POS) Tagging:** Identifying parts of speech (e.g., noun, verb).
* **Stopword Removal:** Eliminating common words with little semantic value (e.g., "the", "a", "is").
* **Stemming & Lemmatization:** Reducing words to their root form.
* **Named Entity Recognition (NER):** Identifying and categorizing entities like names, places, and organizations.

### 4. Vectorization Techniques
This is the process of converting text into numerical vectors.
* **Basic:** Bag of Words, One-Hot Encoding, Count Vectorizer.
* **Statistical:** **TF-IDF** (Term Frequency-Inverse Document Frequency).
* **Word Embeddings:** `Word2Vec`, `GloVe`, `FastText`.
* **Document Embeddings:** `Doc2Vec`.
* **Advanced:** **Sentence Transformers** (e.g., `BERT`-based models).

### 5. Similarity Metrics
Once data is vectorized, similarity metrics are used to measure how close or related two vectors are.
* **Vector Similarity Measures:**
    * Euclidean Distance
    * Dot Product Similarity
    * Cosine Similarity (most common for text)
    * Jaccard Similarity
    * Manhattan Distance

### 6. Implementing Production AI Applications
There are several approaches to building AI applications.
* **Prompts:** Carefully crafting inputs to guide the model's output.
* **Retrieval-Augmented Generation (RAG):** Providing the model with relevant documents or data to answer questions based on that specific context.
* **Pre-training:** Pre-training a foundation model on a custom dataset for more accurate, domain-specific results.

### 7. Large Language Models (LLMs)
LLMs are the core engine behind many modern AI applications.
* **Foundation Models:** Pre-trained by major companies on vast, general-purpose data. They have billions of parameters (e.g., `7b`, `13b`, `70b`, `100b+`).
* **Fine-tuning:** Customizing a base model on your specific dataset.
* **Inference Model:** The model deployed in a production environment.
* **Quantization:** A lossy compression technique to reduce model size (e.g., from 16-bit to 8-bit or 4-bit precision).
* **Tokenization:** The process of breaking text into smaller pieces (tokens) that the model can process. This is often how model usage is billed.
* **Inference Parameters:**
    * **Temperature:** Controls randomness.
    * **Top K / Top P:** Control the selection of next tokens.
    * **Response Length:** Maximum length of the output.
    * **Penalties:** Discourage repetition.
    * **Stop Sequences:** Sequences that signal the model to stop generating text.
* **Popular LLMs:** `OpenAI`, `Claude`, `DeepSeek`, `Mistral`, `Llama`, `Qwen`, `Gemini`, `Grok`, `Phi`.
* **Deployment Tools:** `Llama.cpp`, `Ollama`.
* **Production Platforms:** `OpenAI API`, `Azure`, `Amazon Bedrock`, `Google Vertex AI`, `AWS EC2`, `EKS`, `Azure AKS`, and on-premise solutions.
* **Frontend Chat UIs:** `open-webui`, `librechat`.
* **RAG Frameworks:** `LangChain`, `LlamaIndex`.
* **AI Agents:** `PydanticAI`, `LangGraph`.
* **Fine-tuning Libraries:** `unsloth`.
* **Monitoring & Observability:** Tools like `LiteLLM` and `LangSmith` can track token usage, latency, error rates, prompt changes, retrieval quality, and user feedback.

### 8. Strategies for Reliable AI
Ensuring the accuracy and reliability of AI outputs is crucial.
* **Cite Sources Explicitly:** When using RAG, make the model cite the sources of its information.
* **Verifier Model:** Use a second LLM to fact-check the output of the primary model.
* **Cross-Model Consistency:** Compare outputs from multiple models to identify inconsistencies.
* **Advanced Prompting:** Employ sophisticated prompting techniques to guide the model.
* **Confidence Scores & Temperature Tweaks:** Use the model's confidence scores and adjust the temperature to manage output variability.
* **Human in the Loop:** Involve subject matter experts to review outputs and provide feedback.
* **Monitoring and Analytics:** Continuously monitor user feedback and re-index data to keep the system current.