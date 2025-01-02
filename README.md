# Langchain-PGVector
## Overview
This notebook demonstrates the integration of TensorFlow Hub's Universal Sentence Encoder with a PostgreSQL PGVector database. It facilitates the addition of documents to the vector store, similarity searches, and query retrieval.

---

## Features
1. **Embedding Model**
   - Leverages TensorFlow Hub's Universal Sentence Encoder to generate embeddings for queries and documents.

2. **PostgreSQL Vector Store**
   - Uses PGVector to store and retrieve document embeddings with metadata.

3. **Similarity Search**
   - Supports similarity-based document retrieval based on a query.

4. **Retriever**
   - Transforms the vector store into a retriever for query-based results.

---

## Prerequisites
1. **Python Environment**
   - Install Python 3.8 or later.

2. **Dependencies**
   Install the required Python packages:
   ```bash
   pip install tensorflow-hub numpy langchain-postgres langchain-core
   ```

3. **Database Configuration**
   - Ensure access to a PostgreSQL database with PGVector extension enabled.

---

## How to Use the Notebook

### 1. Initialize the Embedding Model
The `EmbeddingModel` class loads the Universal Sentence Encoder and provides methods to embed queries and documents.
```python
embeddings = EmbeddingModel()
```

### 2. Define the PostgreSQL Connection String
Specify the connection string and collection name for the PGVector database.
```python
conn_string = "<your_connection_string>"
collection_name = "example_documents"
```

### 3. Initialize the PGVector Store
Set up the vector store using the embedding model and connection string.
```python
vector_store = PGVector(
    embeddings=embeddings,
    collection_name=collection_name,
    connection=conn_string,
    use_jsonb=True,
)
```

### 4. Add Documents to the Vector Store
Add documents with content and metadata to the vector store.
```python
docs = [
    Document(page_content="Document content.", metadata={"id": 1, "topic": "example"})
]
vector_store.add_documents(docs, ids=[doc.metadata["id"] for doc in docs])
```

### 5. Perform a Similarity Search
Query the vector store to find similar documents.
```python
query_text = "Your query here"
results = vector_store.similarity_search(query=query_text, k=3)
```

### 6. Use the Retriever
Transform the vector store into a retriever for advanced querying.
```python
retriever = vector_store.as_retriever(search_type="similarity", search_kwargs={"k": 1})
retriever_results = retriever.invoke(query_text)
```

---

## Output Examples
### Similarity Search Results
```plaintext
--- Query Results ---
* Document 1 content [metadata]
* Document 2 content [metadata]
```

### Retriever Results
```plaintext
--- Retriever Results ---
* Document content [metadata]
```

---

## Notes
1. Ensure that your PostgreSQL database is accessible and properly configured.
2. Adjust the connection string to match your database credentials.
3. Update document content and metadata based on your use case.

---

## License
This project is licensed under the MIT License.

