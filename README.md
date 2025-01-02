# Langchain-PGvector
## PGVector Testing

This repository contains a Python notebook designed to test and demonstrate the capabilities of the PGVector vector store, implemented using the LangChain abstraction and PostgreSQL with the `pgvector` extension.

## Overview

PGVector is a vector database extension for PostgreSQL, which can be used to store and retrieve high-dimensional vector embeddings efficiently. This implementation leverages LangChain's integration to provide a seamless interface for managing and querying vectors.

## Features

- **Vector Management**: Add, delete, and query vectors in the PostgreSQL database.
- **Document Retrieval**: Perform similarity searches with optional metadata filtering.
- **Integration with LangChain**: Utilize LangChain's `PGVector` abstraction for enhanced functionality.

## Requirements

Ensure the following dependencies are installed:

- Python 3.8+
- PostgreSQL with `pgvector` extension
- Required Python packages:

  ```bash
  pip install langchain_postgres langchain-openai
  ```

## Setup

### Step 1: Spin Up a PostgreSQL Container

Run the following command to start a PostgreSQL container with the `pgvector` extension enabled:

```bash
docker run --name pgvector-container \
  -e POSTGRES_USER=langchain \
  -e POSTGRES_PASSWORD=langchain \
  -e POSTGRES_DB=langchain \
  -p 6024:5432 \
  -d pgvector/pgvector:pg16
```

### Step 2: Configure OpenAI API Key

Set your OpenAI API key in the environment variable `OPENAI_API_KEY` to enable embedding generation.

```bash
export OPENAI_API_KEY="your-openai-api-key"
```

### Step 3: Run the Notebook

Clone this repository and execute the notebook script provided in `pgvector_testing.py`.

## Usage

### Adding Documents

The script demonstrates how to add documents to the vector store:

```python
from langchain_core.documents import Document

vector_store.add_documents([
    Document(page_content="Sample text", metadata={"id": 1, "topic": "example"})
])
```

### Querying the Vector Store

Perform similarity searches with optional metadata filters:

```python
results = vector_store.similarity_search(query="example", k=5)
for doc in results:
    print(doc.page_content)
```

### Using as a Retriever

Transform the vector store into a retriever for integration with chains or agents:

```python
retriever = vector_store.as_retriever(search_type="similarity", search_kwargs={"k": 3})
retriever_results = retriever.invoke("query text")
```

## Contributing

Contributions are welcome! If you encounter issues or have suggestions, please create an issue or submit a pull request.


