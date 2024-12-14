# Retrieval Augmented Generation for Legal Journals

## Overview

This project explores the use of **Retrieval Augmented Generation (RAG)** to streamline the legal research process. The system integrates advanced retrieval techniques, embedding generation, and summarization to retrieve and summarize relevant legal cases effectively. The goal is to address challenges in legal research, such as ensuring accuracy and avoiding hallucinated references, by grounding the retrieval process in verifiable data.

The system was developed using the **Caselaw Access Project** dataset, focusing on Alaska court cases, and evaluates two retrieval models: **ColBERT (Contextualized Late Interaction over BERT)** and **FAISS (Facebook AI Similarity Search)**, paired with **BART** for summarization.

---

## Directory Structure 

```plaintext
Framework-Insight-Retrieval-and-Summarization/
├── data/
│   ├── Legal_Case_Indexing_Querying.ipynb      # Jupyter Notebook for data indexing and querying
│   ├── faiss_embeddings.npy                    # FAISS embeddings
│   ├── colbert_embeddings.npy                 # ColBERT embeddings
│   ├── legal_cases_index.faiss                # FAISS index file
│   ├── metadata.json                          # Metadata for Colbert
│   ├── metadata_faiss.json                    # Metadata aligned with FAISS
│   ├── output_cases.csv                       # Retrieved results and evaluation output for EDA
│
├── eda/
│   ├── eda.ipynb                              # Exploratory Data Analysis notebook
│
├── json/
│   ├── *.json                                 # Raw legal case files (e.g., 0001-01.json) (**Kept them limited to run**)
│   ├── ideas.qmd                              # Additional notes or ideas for the project
│
├── tables/
│   ├── 1892-03-08_enhanced.png                # Example visualization
│   ├── full_table_enhanced.png                # Enhanced table visualization
│   ├── full_table_long_format.png             # Long-format table visualization
│   ├── What_about_Hillyer_enhanced.png        # Query results visualization
│   ├── What_about_cases_in_Alaska_enhanced.png# Query results visualization
│   ├── Case_on_McIntosh_enhanced.png          # Query results visualization
│   ├── United_enhanced.png                    # Query results visualization
│
├── colbert.ipynb                              # Implementation of ColBERT retrieval
├── faiss.ipynb                                # Implementation of FAISS retrieval
├── colbert.md                                 # Documentation for ColBERT
├── faiss.md                                   # Documentation for FAISS
├── README.md                                  # Project description and details
├── report.pdf                                 # Final report for the project
├── Project Report - Sheeba, Shingai, Powell.docx  # Original project report in Word format
├── Forbes Screenshot.png                      # Supporting screenshot

```
----

## Usage

1. To run ColBERT:
   `colbert.ipynb`
2. To run FAISS
   `faiss.ipynb`

---

## Features

1. **Preprocessing and Metadata Generation**:
   - Normalized case metadata (e.g., case names, abbreviations, decision dates).
   - Segmented long legal opinions into manageable passages (300–512 characters).

2. **Semantic and Metadata-Based Search**:
   - Supports query types such as case names, abbreviations, decision dates, and custom queries.
   - Fuzzy matching for partial matches.

3. **Retrieval Models**:
   - **FAISS**: Optimized for scalability and high-speed similarity search.
   - **ColBERT**: Provides fine-grained, token-level semantic matching.

4. **Summarization with BART**:
   - Generates concise, coherent summaries of retrieved cases.

5. **Evaluation Metrics**:
   - Precision@k, Recall@k, F1-score@k, nDCG@k, and Mean Average Precision (MAP) for assessing retrieval accuracy and ranking quality.

---

## Workflow

1. **Data Preparation**:
   - Extracted legal case data from JSON files.
   - Standardized metadata and segmented long texts for embedding generation.

2. **Embedding Generation**:
   - Used **DistilBERT** to create dense vector embeddings.
   - Token-level embeddings for ColBERT to enable nuanced legal term matching.

3. **Indexing**:
   - Indexed embeddings with FAISS for scalable similarity search.
   - ColBERT embeddings stored for token-level interaction.

4. **Query System**:
   - Normalizes and processes user queries for semantic or metadata-based search.
   - Retrieves top results ranked by similarity.

5. **Summarization**:
   - BART generates concise summaries of retrieved legal documents.

6. **Evaluation**:
   - Compared retrieval models using accuracy metrics across multiple query types.

---

## Key Components 

## Retrieval Techniques

- **ColBERT**: Employs token-level embeddings for fine-grained matching, ensuring highly accurate and context-aware retrieval of legal documents.
- **FAISS**: Uses vector-based retrieval for scalability and fast querying of large datasets.

## Summarization

- **BART**: Processes retrieved documents to produce concise, coherent summaries, aiding in quick comprehension of legal texts.

--- 

## Evaluation Metrics

The system is evaluated using:

- **Precision@k**: Measures relevant documents retrieved in top-\(k\) results.
- **Recall@k**: Proportion of all relevant documents retrieved.
- **F1-score@k**: Harmonic mean of precision and recall.
- **nDCG@k**: Evaluates ranking quality.
- **MAP**: Reflects overall retrieval performance.

---

## Results

- **ColBERT** consistently outperformed **FAISS** in recall, F1-score, and ranking quality (nDCG@k).
- FAISS required re-indexing for new data, whereas ColBERT scaled dynamically.
- BART provided effective summaries for both retrieval systems, ensuring actionable insights.

---

## Limitations 

1. **Summarization Constraints**: BART’s input window may truncate lengthy documents, omitting critical details.
2. **Scalability Issues**: FAISS requires re-indexing for new data, while ColBERT’s token-level matching is computationally expensive.
3. **Domain-Specific Nuances**: Pre-trained models like DistilBERT and ColBERT may not fully capture legal terminology.
4. **Metadata Quality**: The system heavily relies on complete and consistent metadata.


