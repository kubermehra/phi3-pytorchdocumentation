## PyTorch Documentation RAG System

This repository contains a system that leverages Knowledge Graphs (KG) and Vector Stores with a Large Language Model (LLM) to provide contextual answers from PyTorch documentation. The system autonomously extracts information, stores it efficiently, and combines retrieval methods for enhanced question-answering capabilities.

Key functionalities include:
* **Entity Triple Extraction**: Utilizing an LLM to extract entity relationships from text.
* **Knowledge Graph Storage**: Storing extracted triples in a knowledge graph (Neo4j or in-memory).
* **Vector Database Storage**: Storing document chunks in a FAISS vector database.
* **Hybrid Querying**: Querying both the knowledge graph and the vector database based on user prompts.
* **Contextual Answer Generation**: Feeding retrieved facts and documents to an LLM (Phi-3) for more accurate and contextually rich answers.

### Project Structure

The repository is organized into several Jupyter notebooks and data files, each serving a specific purpose in the RAG pipeline:

* `KG_RAG_Autonomous_Fixed.ipynb`: The main notebook demonstrating the end-to-end RAG pipeline, including LLM integration, knowledge graph interactions, and vector store querying.
* `README.md`: This README file.
* `data.json`: Contains raw PyTorch documentation snippets and information, likely used as source material.
* `doc_info.json`: Contains structured information about the PyTorch documentation, parsed from the raw data.
* `environment.yml`: Specifies the Conda environment dependencies required to run the project.
* `extraction.ipynb`: Details the web scraping process used to extract data from PyTorch documentation, likely generating `data.json` or `doc_info.json`.
* `nosql_db_load.ipynb`: Demonstrates how to load the processed document information into a MongoDB database.
* `rag.ipynb`: Focuses on the RAG pipeline components, including setting up the LLM, embedding model, vector store, and retrieval chain.
* `rag_test_cases.ipynb`: A placeholder for test cases related to the RAG implementation.
* `vector_embedding.ipynb`: Illustrates the process of chunking documents, generating embeddings, and saving the FAISS vector store.

### Extraction Steps from PyTorch Documentation using Beautiful Soup

The `extraction.ipynb` notebook outlines the process of extracting documentation content from the PyTorch website using Python's `requests` and `BeautifulSoup` libraries.

Here's a summary of the extraction steps:

1.  **Define Target URLs**: A list of URLs pointing to specific sections of the PyTorch documentation (e.g., `torch.nn` modules) is defined as the scraping targets.
2.  **Fetch HTML Content**: For each URL, an HTTP GET request is made using the `requests` library to fetch the raw HTML content of the webpage.
    ```python
    import requests
    from bs4 import BeautifulSoup

    url = "[https://pytorch.org/docs/stable/nn.html](https://pytorch.org/docs/stable/nn.html)"
    response = requests.get(url)
    html_content = response.text
    ```
3.  **Parse HTML with Beautiful Soup**: The fetched HTML content is then parsed by `BeautifulSoup` to create a parse tree, which allows for easy navigation and searching of the HTML elements.
    ```python
    soup = BeautifulSoup(html_content, 'html.parser')
    ```
4.  **Identify and Extract Relevant Sections**: The notebook typically identifies specific HTML tags and attributes (e.g., `<p>`, `<h1>`, `<a>` within certain `div` elements) that contain the documentation text, titles, and links.
    * **Extracting Text Content**: Iterates through paragraphs (`<p>`) or other text-containing tags within the main content area of the documentation page.
    * **Extracting Titles**: Identifies heading tags (`<h1>`, `<h2>`, etc.) to capture the section titles.
    * **Extracting Links**: Finds `<a>` tags to extract hyperlinks, which might point to sub-sections or related documentation.
5.  **Process and Structure Data**: The extracted text, titles, and links are then processed. For instance, newlines and extra spaces are cleaned up. The information is typically structured into a dictionary or similar data structure for each document or section.
6.  **Store Extracted Data**: The structured data is then saved, often in a JSON format. In this project, it appears that the extracted information is saved to `data.json` and/or `doc_info.json`, serving as the primary data source for the RAG system.
    ```python
    import json
    # Example of saving extracted data
    with open('doc_info.json', 'w', encoding='utf-8') as f:
        json.dump(extracted_data, f, ensure_ascii=False, indent=4)
    ```

This extraction process forms the foundation for building the knowledge graph and vector store, enabling the RAG system to retrieve relevant information from the PyTorch documentation.
