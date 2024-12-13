# Legal Case Retrieval System: Workflow Documentation on Colbert

## Overview
The Legal Case Retrieval System efficiently retrieves and summarizes legal cases based on user queries. It supports multiple query types, including names, abbreviations, decision dates, jurisdictions, and custom legal queries. This document outlines the workflow implemented in the system, including the natural language processing (NLP) and language modeling (LM) techniques used.

---

## Workflow Components

### 1. Data Preprocessing
- **Objective**: Standardize and prepare legal case metadata for efficient query handling.
- **Steps**:
  1. **Loading JSON Data**: Case data is loaded from JSON files in a specified directory.
  2. **Standardizing Fields**:
     - **Name and Abbreviation**: Convert to lowercase, remove special characters, and normalize legal case names (e.g., `v.` becomes `v`).
     - **Decision Date**: Standardize dates into the `YYYY-MM-DD` format for consistency.
     - **Cleaned Text**: Extract detailed text from `casebody` for summarization purposes.
  3. **Saving Metadata**: The processed data is saved in a metadata JSON file for retrieval operations.

---

### 2. Query Handling
- **Objective**: Process user queries and determine the appropriate retrieval method.
- **Steps**:
  1. **User Query Input**:
     - The user selects a query type from the following options:
       - Search by Name
       - Search by Abbreviation
       - Search by Decision Date
       - Search by Jurisdiction
       - Custom Legal Query
  2. **Query Preprocessing**:
     - Normalize queries by:
       - Removing filler words (e.g., "What about," "Tell me about").
       - Standardizing "v." and "vs" for consistency.
     - Extract specific data (e.g., `YYYY-MM-DD` for decision dates).

---

### 3. Retrieval Logic
- **Objective**: Retrieve cases based on the query type.
- **Steps**:
  1. **Decision Date Matching**:
     - Extract the date from the query using regex.
     - Match the extracted date with the `decision_date` field in the metadata.
  2. **Name and Abbreviation Matching**:
     - Normalize and compare the query with `name` and `abbreviation` fields in the metadata.
     - Support both exact and partial matches.
  3. **Custom Query Handling**:
     - Fallback to name and abbreviation matching if no specific query type is identified.

---

### 4. Result Display and Summarization
- **Objective**: Present retrieved cases to the user and generate summaries if requested.
- **Steps**:
  1. **Display Retrieved Results**:
     - List all matching cases with indices, showing key metadata fields such as:
       - Case Name
       - Decision Date
       - Jurisdiction
  2. **Summarization Prompt**:
     - Ask the user if they want to summarize the results.
     - Allow users to specify which results to summarize (e.g., "1", "2", or "all").
  3. **Generate Summary**:
     - Use a pre-trained summarization model to summarize the text from the selected cases.

---

## Language Modeling (LM) Techniques

### 1. Retrieval Techniques
- **Dense Embedding-Based Retrieval**:
  - **Technique**: Dense retrieval using ColBERT (Contextualized Late Interaction over BERT).
  - **Details**:
    - ColBERT leverages BERT embeddings to create dense vector representations of documents and queries.
    - Interaction-based scoring is used to compute the similarity between the query and document embeddings.
    - Query embeddings are generated dynamically for retrieval, ensuring relevance and semantic alignment.

### 2. Embedding Techniques
- **Model Used**: `bert-base-uncased` from HuggingFace.
- **Process**:
  - Textual data is tokenized into subword units.
  - Contextualized embeddings are generated for each token using the BERT model.
  - Attention mechanisms allow BERT to consider the context of each token within the input sequence.
  - The embeddings are fine-tuned for relevance scoring.

### 3. Retrieval-Augmented Generation (RAG)
- **Technique**: RAG combines dense retrieval with sequence generation.
- **Details**:
  - **Retriever**: FAISS (Facebook AI Similarity Search) is used as the dense retriever for finding relevant documents.
  - **Generator**: A transformer-based sequence-to-sequence model generates answers by conditioning on retrieved documents.
  - The retrieval and generation processes are integrated to handle open-domain questions effectively.
- **Use Case**:
  - Applied for summarization tasks where the system uses retrieved legal case text as input for the generator.

### 4. Summarization Techniques
- **Model Used**: `facebook/bart-large-cnn`.
- **Process**:
  - The retrieved case text is concatenated into a single input context.
  - BART tokenizes the input and processes it using an encoder-decoder transformer architecture.
  - The model generates a summary by decoding the encoder's output, balancing brevity and informativeness.
- **Advantages**:
  - Handles long input sequences effectively, making it suitable for legal text.
  - Pre-trained on a large dataset for abstractive summarization tasks.

---

## Example User Interaction

### Query: Custom Query with Decision Date
- **Input**: `What about cases on 1901-11-16?`
- **Workflow**:
  1. Extract the date `1901-11-16`.
  2. Match `decision_date` in the metadata.
  3. Display the matching cases.
  4. Prompt the user to select cases for summarization.

- **Output**:

- **Retrieved Results**:

1. u s ex rel mcintosh v price et al (Decision Date: 1901-09-19, Jurisdiction: Alaska)
2. price et al v mcintosh et al (Decision Date: 1901-11-16, Jurisdiction: Alaska)

---

## Key Features
1. **Flexible Query Types**:
 - Supports names, abbreviations, decision dates, jurisdictions, and custom queries.
2. **Dynamic Summarization**:
 - Allows the user to choose specific cases for summarization or summarize all retrieved results.
3. **Advanced Retrieval**:
 - Dense retrieval with embeddings ensures semantic relevance.
4. **Powerful Summarization**:
 - Transformer-based abstractive summarization handles lengthy legal text effectively.
5. **Robust Query Normalization**:
 - Handles filler words, case insensitivity, and legal formatting conventions.

---

## Conclusion
The Legal Case Retrieval System combines dense retrieval, RAG, and transformer-based summarization to provide a robust and user-friendly solution for querying and summarizing legal cases. Leveraging state-of-the-art NLP techniques, the system ensures semantic accuracy, flexibility, and scalability for legal text analysis.
